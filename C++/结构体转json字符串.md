## 方法1
````C++
#include "lib/rapidjson/document.h"
#include "lib/rapidjson/writer.h"
#include "lib/rapidjson/stringbuffer.h"

Document document;
Document::AllocatorType& allocator = document.GetAllocator();
document.SetObject();

document.AddMember("id", Value().SetInt(m_patient.id), allocator);
document.AddMember("name", Value().SetString(m_patient.name.c_str(),m_patient.name.size()), allocator);
document.AddMember("sex", Value().SetBool(m_patient.sex), allocator);
document.AddMember("birthday", Value().SetString(m_patient.birthday.c_str(),m_patient.birthday.size()), allocator);

//生成字符串
StringBuffer buffer;
Writer<rapidjson::StringBuffer> writer(buffer);
document.Accept(writer);

return buffer.GetString();
````

## 方法2
````C++
StringBuffer buffer;
Writer<StringBuffer> writer(buffer);
writer.StartObject();
writer.Key("id");
writer.Int(m_patient.id);
writer.Key("name");
writer.String(m_patient.name.c_str());
//writer.
writer.EndObject();

return buffer.GetString();
````

## 参考链接
https://blog.csdn.net/u014449046/article/details/79070301