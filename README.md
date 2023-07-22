# skip-mdm
skip mac mdm note


1.	12系统，开机不连网（wifi连接时选择跳过）进入系统，能绕过MDM激活系统（12系统bug，13系统已修复，13系统也能绕过，但非常麻烦，建议使用12系统绕过）。
2.	若不小心联网，则需求恢复系统（从苹果服务器下载系统，下载完重启时断开路由器网线）
3.	若已升级到高版本系统，抹掉硬盘，此时苹果服务器恢复系统已不是12系统，需用硬盘/u盘安装12系统（网上有很多硬盘安装macos的资料），再使用上述1的方法绕过激
4.	激活后，关闭MDM弹窗提示“”“”“”“”“重要重要重要，否则弹窗烦死人”“”“”“”“”“”“”
	4.1	关闭SIP：关机状态长按启动键，进入恢复模式，执行命令 crsutil disable   重启电脑（reboot命令）
	4.2	删除并创建假profile文件：
  	
		sudo rm /var/db/ConfigurationProfiles/Settings/.cloudConfigHasActivationRecord
		sudo rm /var/db/ConfigurationProfiles/Settings/.cloudConfigRecordFound
		sudo touch /var/db/ConfigurationProfiles/Settings/.cloudConfigProfileInstalled
		sudo touch /var/db/ConfigurationProfiles/Settings/.cloudConfigRecordNotFound

	4.3	执行sudo profiles show -type enrollment  测试是否删除成功（若报错表示删除成功）

	4.4	执行 sudo launchctl disable system/com.apple.ManagedClient.enroll  该命令可防止重启后系统自动恢复以上删除的profile（重要）
	4.5	以防万一，可将以下域名屏蔽，使apple不能恢复mdm profile。将以下指向添加到 /etc/hosts中
  	
		#block mdm connect
		0.0.0.0 iprofiles.apple.com
		0.0.0.0 mdmenrollment.apple.com
		0.0.0.0 deviceenrollment.apple.com
		0.0.0.0 gdmf.apple.com
		0.0.0.0 acmdm.apple.com
		0.0.0.0 albert.apple.com

6.	做完以上操作，不再会弹出监管注册弹框，为确保系统使用安全，建议恢复SIP：关机状态长按启动键，进入恢复模式，执行命令 crsutil enable   重启电脑。


		
