CRB版本发布

```
release版本
	release版本一般是正式发布版本，编译器会对代码进行优化，以提高执行效率，移除了调试信息，减少了文件大小，提高了运行速度，用于产品发布和部署，是终端用户最终使用的版本。

debug版本
	Debug版本是供开发者在开发过程中使用的版本，用于调试和问题定位。包含完整的调试信息，运行速度通常比Release版本慢，主要用于开发和测试阶段，帮助开发者找出程序中的错误和性能问题。
```

B800版本

```
http://space.jaguarmicro.com/pages/viewpage.action?pageId=96978432

固件版本
imu:
	https://jfrog1.jaguarmicro.com/artifactory/corsica-soc-generic-local/release/release_v2.6.0_bugfix0129/package/crb/imu_flash_crb_5.img
scp:
	https://jfrog1.jaguarmicro.com/artifactory/corsica-soc-generic-local/release/release_v2.6.0_bugfix0129/package/crb/scp_flash_crb_5_n2_2.0g_cmn_1.65g.img

业务版本imu
	https://jfrog1.jaguarmicro.com/artifactory/corsica-sw-generic-local/release/imu-version/ubuntu/corsica_1.0.0.B800/crb_5_imu_flash_202401302200.img

业务版本：
	https://jfrog1.jaguarmicro.com:443/artifactory/corsica-sw-generic-local/release/full-version/anolis/corsica_1.0.0.B800/corsica_1.0.0.B800_jmnd_rel.tar.gz
```

每日构建获取最新日期界面（每日构建跑的都是debug版本）

```
https://jfrog1.jaguarmicro.com/ui/native/corsica-sw-generic-local/snapshot/full-version/corsica_dpu_dev/anolis/

[cloudSupportVersionFileConfig]
searching_url = https://jfrog1.jaguarmicro.com:443/artifactory/corsica-sw-generic-local/snapshot/full-version/corsica_dpu_dev/anolis/
filePattern = https://jfrog1.jaguarmicro.com/artifactory/corsica-sw-generic-local/snapshot/full-version/corsica_dpu_dev/anolis/@{download_date}/@{download_date}_jmnd.tar.gz
relFilePattern = https://jfrog1.jaguarmicro.com/artifactory/corsica-sw-generic-local/snapshot/full-version/corsica_dpu_dev/anolis/@{download_date}/@{download_date}_jmnd_rel.tar.gz

[imuVersionFileConfig]
searching_url = https://jfrog1.jaguarmicro.com:443/artifactory/corsica-sw-generic-local/snapshot/imu-version/corsica_dpu_dev/ubuntu/
filePattern = https://jfrog1.jaguarmicro.com/artifactory/corsica-sw-generic-local/snapshot/imu-version/corsica_dpu_dev/ubuntu/@{download_date}/crb_5_imu_flash_@{download_date}.img

[scpVersionFileConfig]
searching_url = https://jfrog1.jaguarmicro.com:443/artifactory/corsica-soc-generic-local/snapshot/daily-build/
filePattern = https://jfrog1.jaguarmicro.com/artifactory/corsica-soc-generic-local/snapshot/daily-build/@{download_date}/package/crb/scp_flash_crb_5_n2_2.0g_cmn_1.65g.img

```

B740稳定版本

```
scp链接
https://jfrog1.jaguarmicro.com/artifactory/corsica-soc-generic-local/release/release_v2.5.1_bugfix0122/package/crb/scp_flash_crb_5_n2_2.0g_cmn_1.65g.img
imu链接
https://jfrog1.jaguarmicro.com/artifactory/corsica-sw-generic-local/release/imu-version/ubuntu/corsica_1.0.0.B740/crb_5_imu_flash_202401221534.img
业务版本
https://jfrog1.jaguarmicro.com:443/artifactory/corsica-sw-generic-local/release/full-version/anolis/corsica_1.0.0.B740/corsica_1.0.0.B740_jmnd_rel.tar.gz
```

