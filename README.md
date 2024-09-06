## 模块信息
- 名称：时钟脚本
- 作者：7e1ce9fb
- 功能：基于小米时钟和 Xposed 实现的定时执行脚本功能

> ⚠️ **目前仅适用于小米手机**

## 本模块的一些情况说明
1. 本模块脚本执行前提条件，你需要授权小米时钟 root 权限，不然会出现执行失败；
2. 本模块已在小米时钟 15.29.0 版本测试可用，其他的版本兼容性未知自行测试；
3. 本模块要求小米时钟的闹钟备注的脚本必须为 .sh 结尾，否则会当作普通闹钟备注；
4. 小米时钟中备注支持绝对路径，若为相对路径则在 /data/adb/alarm_shell/scripts 目录下查找；
5. 目前脚本任务执行实行队列机制，前一个任务未执行结束，后一个来的任务则进入排队等待状态；
6. 如果需要支持关机闹铃脚本，有屏幕锁的可通过 MT 管理器修改 /data/adb/alarm_shell 目录下 password 文件，填入你的锁屏密码（只支持PIN和混合密码方式），
   关机闹铃会尝试解锁屏幕 3 次，解锁成功后会立即锁屏，这么做防止人睡觉时手机被偷窥狂魔偷窥；
7. 关于为什么要输入锁屏密码才执行脚本的解释，因为现在的高版本的手机开机重启后未进行至少一次屏幕解锁，手机 data 分区会处于加密保护状态无法读取，
   其次经测试在我手机上 service 后台执行任务第一次开机运行必须要模拟用户至少启动过一次应用界面，否则对于长时间的后台任务会存在被系统杀死的情况，
   由于无屏幕锁情况判断比较困难为了兼容无屏幕锁情况，开机十分钟内执行任何脚本都会尝试解锁屏幕；
8. 关于执行任务过程中屏幕频繁唤醒的解释，系统底层对于熄屏情况会严重限制后台任务的IO等一系列操作，这么做旨在保证任务（如备份手机脚本需要长时间执行）的顺利完成，任务全部执行结束后会自动取消唤醒屏幕进入熄屏状态。