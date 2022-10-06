# ubuntu20_ros_noetic_gluon_error-no-matching-function-for-call-to-ros_control_boilerplate-GenericHW
## 问题描述
ros编译出现
```
error: no matching function for call to

‘ros_control_boilerplate::GenericHWControlLoop::GenericHWControlLoop

(ros::NodeHandle&, boost::shared_ptr<gluon_control::GluonHWInterface>&)’
```
### 如下图

![image](https://github.com/OTT123/ubuntu20_ros_noetic_gluon_error-no-matching-function-for-call-to-ros_control_boilerplate-GenericHW/blob/main/pic/gluon_std_ptr.png)

## 原文连接
>版权声明：本文为CSDN博主「hyxxi」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
>
>原文链接：https://blog.csdn.net/hyxxi/article/details/124561651

## 原因
ros noetic版本中，第二个参数由boost::shared_ptr改为std::shared_ptr。

## 解决方法
```
template<typename T>
void do_release(typename boost::shared_ptr<T> const&, T*)
{
}
template<typename T>
typename std::shared_ptr<T> to_std(typename boost::shared_ptr<T> const& p)
{
    return
        std::shared_ptr<T>(
                p.get(),
                boost::bind(&do_release<T>, p, _1));
 
}
```
在需要类型转换的地方先定义一个std::shared_ptr类型 再调用to_std函数即可解决。

### tips 将传递的参数在定义时的boost::shared_ptr直接修改为std::shared_ptr也可。
