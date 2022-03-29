## wpf 验证方式
- ExceptionValidationRule 对应接口无，检查在更新数据源source时抛出异常，默认false。
- DataErrorValidationRule 对应接口IDataErrorInfo，检查IDataErrorInfo接口对象的生成错误，默认为false。
- NotifyDataErrorValidation 对应接口INotifyDataErrorInfo，检查INotifyDataErrorInfo接口对象生成的错误。默认为true。

默认ValidationStep属性为RawProposedValue，对于DataErrorValidationRule默认为UpdatedValue

## Validation处理过程
数据验证发生在TwoWay和OneWayToSource的Binding中
验证流程（如果有一个错误发生，验证流程终止）

1. 绑定引擎（Binding engine）检查ValidationStep设置为RawProposedValue的任意自定义ValidationRule

2. 绑定引擎调用Converter（如果有Converter）

3. 如果转换成功，绑定引擎检查ValidationStep设置为ConvertedProposedValue的任意自定义ValidationRule

4. 绑定引擎设置source属性

5. 绑定引擎调用Validate方法，检查ValidationStep设置为UpdatedValue的任意自定义ValidaRule。

    1. 如果Binding中包含DataErrorValidationRule，并且ValidationStep设置为Default（UpdatedValue），此时检查DataErrorValidationRule

    2. 此时也是ValidatesOnDataErrors设置为true的默认检查时机

6. 绑定引擎调用Validate方法检查ValidationStep设置为CommittedValue的ValidationRule


## 参考
1. https://blog.csdn.net/leftxden/article/details/89325048