# VISA的简单使用记录

## 背景
上位机连接示波器的时，只能使用USBTMC串口方式进行通信，所以需要按照NI VISA进行读写数据处理。

## 安装VISA
[NI VISA下载地址](https://www.ni.com/zh-cn/support/downloads/drivers/download.ni-visa.html#346210)  
通过NI VISA MAX，找到设备与接口，然后再打开VISA测试面板进行串口调试  
![image](NI_VISA_RW/NI_MAX.png)  
!<img src="NI_VISA_RW/serialport_message.png" width = 60% height = 60% />  


## QT串口代码
以连接示波器代码为例  
在CMakeLists中添加visa的头文件和库文件如下 
![image](NI_VISA_RW/CMakeLists.png)  

1. 连接串口
````C++
#include <visa.h>

static ViSession defaultRM;
static ViSession instr;
static ViUInt32 numInstrs;
static ViFindList findList;
static ViUInt32 retCount;
static ViUInt32 writeCount;
static ViStatus status;
static char instrResourceString[VI_FIND_BUFLEN];

static unsigned char buffer[100];
static char stringinput[512];

bool oscilloscope::connectUSB(QString ID){
    /*
    * First we must call viOpenDefaultRM to get the manager
    * handle.  We will store this handle in defaultRM.
    */
   status=viOpenDefaultRM (&defaultRM);
   if (status < VI_SUCCESS)
   {
      printf ("Could not open a session to the VISA Resource Manager!\n");
      exit (EXIT_FAILURE);
   }

   /* Find all the USB TMC VISA resources in our system and store the  */
   /* number of resources in the system in numInstrs.                  */
   status = viFindRsrc (defaultRM, "USB?*INSTR", &findList, &numInstrs, instrResourceString);

   if (status < VI_SUCCESS)
   {
      printf ("An error occurred while finding resources.\nHit enter to continue.");
      fflush(stdin);
      getchar();
      viClose (defaultRM);
      //return status;
   }

   /*
    * Now we will open VISA sessions to all USB TMC instruments.
    * We must use the handle from viOpenDefaultRM and we must
    * also use a string that indicates which instrument to open.  This
    * is called the instrument descriptor.  The format for this string
    * can be found in the function panel by right clicking on the
    * descriptor parameter. After opening a session to the
    * device, we will get a handle to the instrument which we
    * will use in later VISA functions.  The AccessMode and Timeout
    * parameters in this function are reserved for future
    * functionality.  These two parameters are given the value VI_NULL.
    */

   for (int i=0; i<numInstrs; i++)
   {
      if (i > 0)
         viFindNext (findList, instrResourceString);
      printf("%d\t%s\n", i, instrResourceString);

      if (QString(instrResourceString) == ID){
          status = viOpen (defaultRM, instrResourceString, VI_NULL, VI_NULL, &instr);
          printf("%d\t%s\n", i, instrResourceString);
          if (status < VI_SUCCESS)
          {
             printf ("Cannot open a session to the device %d.\n", i+1);
          }
          break;
      }
    }
}
````
2. 发送指令，读取buffer
````C++
bool oscilloscope::sendCMD(QString cmd, unsigned char* out_buffer){
    /*
     * At this point we now have a session open to the USB TMC instrument.
     * We will now use the viWrite function to send the device the string "*IDN?\n",
     * asking for the device's identification.
     */
    cmd = cmd+"\n";
    strcpy (stringinput, cmd.toUtf8());
    status = viWrite (instr, (ViBuf)stringinput, (ViUInt32)strlen(stringinput), &writeCount);
    if (status < VI_SUCCESS)
    {
       printf ("Error writing to the device %d.\n", 1);
       status = viClose (instr);
       return false;
    }

    /*
    * Now we will attempt to read back a response from the device to
    * the identification query that was sent.  We will use the viRead
    * function to acquire the data.  We will try to read back 100 bytes.
    * This function will stop reading if it finds the termination character
    * before it reads 100 bytes.
    * After the data has been read the response is displayed.
    */
   status = viRead (instr, buffer, 100, &retCount);
   //float a;
   //viScanf(instr, "%f",&a);
   //viScanf将寄存器中的数据读取出来，使用的是指针，需要注意不要和read混用
   if (status < VI_SUCCESS)
   {
      printf ("Error reading a response from the device %d.\n", 1);
      return false;
   }
   else
   {
      memcpy(out_buffer,buffer,100*sizeof (char));
      printf ("\nDevice %d: %*s\n", 1, retCount, buffer);
      return true;
   }
}
````

3. 关闭串口(注意串口必须关闭)
````C++
void oscilloscope::closeUSB(){
    /*
     * Now we will close the session to the instrument using
     * viClose. This operation frees all system resources.
     */
    status = viClose (defaultRM);
    printf ("Hit enter to continue.");
    fflush (stdin);
}
````

4. 调用过程如下
````C++
connectUSB("USB0::......::INSTR");
sendCMD("*IDN?",buffer);
closeUSB();
````
