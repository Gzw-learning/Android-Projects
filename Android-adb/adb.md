 ###### 一些point：
- V8a为64位系统
- 安卓会逐渐抛弃32位。安卓13可以装32位，安卓14不可以。
- 日志字母代表含义
  *V：明细*
  *D：调试*
  *I：信息*
  *w：警告*
  *E：错误*
  *F：严重错误*
- 快速定义有问题的日志：
  *fatal严重错误：软件崩溃
  error错误：软件无显示*
  - #X86架构和ARM架构 
查看处理器命令：adb shell get prop ro.product.cpu.abi
###### adb命令分类
//shell 是进入安卓系统
==1. adb shell am c o==
am：activity manager
c:  command
o:  option


==2. adb shell pm c==
pm:  package manager
eg:adb shell pm packages 包名
eg：adb shell pm install o p


==3.adb shell dumpsys [service]：一般分析手机问题、运行状态、使用情况==
adb shell dumpsys cpuinfo //cpu使用百分比
adb shell dumpsys package [packagename]//看指定包名信息
adb shell dumpsys activity o c
// c有四种方式展示：1.activities状态，2.processes进程，3.services状态，4.package，5.top 顶层活动的状态
adb shell dumpsys meminfo//显示进程
获取当前页面的信息：adb shell dumpsys activity top

---
###### 测试常用：
  adb devices:列出连接上的设备
  adb -s devices_id：使用指定设备操作
  adb bugreport:将之前所有崩溃抓取（无响应用这个）
  adb shell dumpsys activity activities:获取当前屏幕的apk包名
  adb shell pm path 包名：显示apk文件在手机上的位置
  adb pull 手机上的位置 电脑上的位置：将手机上的文件导入到电脑指定位置
  （adb push是将电脑文件放进手机）
  adb logcat -c：清除手机原有日志
  adb logcat -v time > 电脑txt文档位置：将log导出到电脑该位置
  adb install apk位置：下载apk
  adb install -r xxx.apk:覆盖安装（保留缓存和数据）
  adb uninstall package:卸载
  adb uninstall -k package:卸载时保存数据和缓存目录

  ==adb 批量安装apk：==
  1.把apk放在某一文件夹下（路径不能有中文和空格）
  2.for %i in （路径\*.apk） do adb install %i

  ==多apk应用的apk安装到手机：==
  adb install-multiple apk路径1 apk路径2 apk路径...
  
 ==列出手机apk包名：==
  adb shell pm list package -3:列举第三方包名
  adb shell service list：只列系统服务，不涉及三方
  
  ==重启==
  adb reboot:正常重启
  adb reboot bootloader:重启到bootloader（刷机模式）
  adb reboot recovery:重启到recovery（恢复模式）
  
  ==Monkey测试==
  adb shell pm monkey -p app包名 次数：随机测试
  adb shell monkey -f /sdcard/xxx.script :script文件测试

  ==查看进程==
  adb shell ps:列出进程列表及其pid
  adb shell kill pid：杀死指定的pid进程
  adb shell ps -x pid：查看指定进程信息

  ==查看service==
  adb shell service list

  ==查看内存使用情况==
  adb shell cat /proc/meminfo：查看系统当前内存使用情况
  adb shell dumpsys meminfo package：查看指定包名应用内存使用情况

  ==文件==
  创建文件
  adb shell touch /sdcard/text.txt
  复制文件
  adb shell cp /sdcard/text.txt /sdcard/test/
  移动文件
  adb shell mv /sdcard/1.txt /sdcard/2.txt(移动同一目录下文件相当于重命名文件)
 
#获取进程的pid
1.adb shell ps会显示所有进程的信息，比较粗暴
2.adb shell "ps |grep 包名"（这里包名支持部分匹配）（有时候可能需要加个-ef，可能和机型有关系）
#杀死进程的方法
1.adb shell kill -9 进程号
2.adb shell an force-stop 包名全称（不支持部分匹配，必须全称）
#adb显示错误级别的消息
adb logcat *:E

#安卓按包名打印日志
（window）
adb logcat | findstr 包名 >"D:\myLog2.txt"
（linux或mac）
adb logcat | grep -F "
adb shell ps | grep com.abc.package | cut -c10-15

#如果显示adb空格devices空格unauthorized
关掉手机的debug模式，重新打开就行了（应该是不小心关闭过电脑的测试权限）
