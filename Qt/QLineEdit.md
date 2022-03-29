1. QLineEdit边框可以通过setFrame(false)取消
2. setPlaceHolderText(const QString) 设置它的灰底的默认值，当点击时自动关闭
3. setValidator限制器，设置限制
4. setInputMask，设置掩码，根据设定的信息，限制只能输入指定类型的数据及个数。和setValidator共同使用时，setValidator会失效