up：20240730  awvs.sh 一键更新可用版本，自己改密码呗
证书有效期
Licensed
Expires on Sep 19, 2123
Licensed targets
100000

#!/bin/bash
docker stop $(docker ps -a | grep "awvs" | awk '{print $1}');
chattr -i -R /var/lib/docker/overlay2/*
docker rm $(docker ps -a | grep "awvs" | awk '{print $1}');
docker rmi -f $(docker images | grep "awvs" | awk '{print $3}');
docker rmi --no-prune $(docker images | grep "awvs" | awk '{print $3}');


docker run -it -d \
--name awvs \
-p 3443:3443 \
--restart=always \
xrsec/awvs


curl -s -o awvs_listen.zip https://gh-proxy.com/https://raw.githubusercontent.com/shellsec/awvs_listen/master/awvs_listen02.zip
docker cp awvs_listen.zip awvs:/awvs/
docker exec -it awvs /bin/bash -c "unzip -o /awvs/awvs_listen.zip -d /home/acunetix/.acunetix/data/license/"
docker exec -it awvs /bin/bash -c "chmod 444 /home/acunetix/.acunetix/data/license/license_info.json"
docker exec -it awvs /bin/bash -c "chown acunetix:acunetix /home/acunetix/.acunetix/data/license/license_info.json"
docker exec -it awvs /bin/bash -c "chattr +i /home/acunetix/.acunetix/data/license/license_info.json"

docker exec -it awvs /bin/bash -c "chmod 444 /home/acunetix/.acunetix/data/license/wa_data.dat"
docker exec -it awvs /bin/bash -c "chown acunetix:acunetix /home/acunetix/.acunetix/data/license/wa_data.dat"
docker exec -it awvs /bin/bash -c "chattr +i /home/acunetix/.acunetix/data/license/wa_data.dat"

docker exec -it awvs /bin/bash -c "mv /home/acunetix/.acunetix/data/license/wvsc /home/acunetix/.acunetix/v_*/scanner/"
docker exec -it awvs /bin/bash -c "chmod 777 /home/acunetix/.acunetix/v_*/scanner/wvsc"
docker exec -it awvs /bin/bash -c "chown acunetix:acunetix /home/acunetix/.acunetix/v_*/scanner/wvsc"

docker exec -it awvs /bin/bash -c "rm /awvs/awvs_listen.zip"
docker exec -it awvs /bin/bash -c "echo '127.0.0.1 updates.acunetix.com' > /awvs/.hosts"
docker exec -it awvs /bin/bash -c "echo '127.0.0.1 erp.acunetix.com' >> /awvs/.hosts"
docker exec -it awvs /bin/bash -c "::1  erp.acunetix.com' >> /awvs/.hosts"
docker exec -it awvs /bin/bash -c "192.178.49.174  telemetry.invicti.com' >> /awvs/.hosts"
docker exec -it awvs /bin/bash -c "2607:f8b0:402a:80a::200e  telemetry.invicti.com' >> /awvs/.hosts"

docker restart awvs
rm -rf awvs_listen.zip





法海已经去找爱了,废弃了
下载说明：https://www.fahai.org<br />
本博客所发布的Docker_Awvs23.x一键安装脚本[支持版本更新]仅限用于学习和研究目的；<br />
感谢：XrSec<br />
当前软件版本：23.6.230628115<br />
【注1：由于服务器连通性原因，可能导致激活失败问题，可尝试重新安装！】<br />
【注2：推荐安装系统 Centos7 / Kali / Ubuntu 】<br />
破解方法：<br />
1、下载 check.sh 文件至服务器内执行或直接使用下面命令<br />
bash &lt;(curl -sLk https://www.fahai.org/aDisk/Awvs/check.sh) xrsec/awvs<br />
或<br />
bash &lt;(curl -sLk https://www.fahai.org/aDisk/Awvs/check.sh) registry.cn-hangzhou.aliyuncs.com/xrsec/awvs<br />

报错了<br />
&nbsp; [✓]&nbsp; Start Install<br />
&nbsp; [✓]&nbsp; The Container awvs Was Deleted Success!<br />
&nbsp; [✓]&nbsp; Docker Pull xrsec/awvs Success!<br />
&nbsp; [✓]&nbsp; Create AWVS container Success!<br />
&nbsp; [✓]&nbsp; The current version is the latest version<br />
&nbsp; [✓]&nbsp; Download awvs_listen.zip Success!<br />
&nbsp; [✗]&nbsp; Unzip awvs_listen.zip failed<br />
假设已经执行到这一步！执行下面即可<br />
docker exec awvs bash -c "cat /awvs/LAST_VERSION | sed 's/ //g' 2&gt;/dev/null"<br />
mkdir /tmp/awvs &gt;/dev/null 2&gt;&amp;1<br />
rm -rf /tmp/awvs_listen.zip<br />
cd /tmp/<br />
git clone https://ghproxy.com/https://github.com/shellsec/awvs_listen.git<br />
mv awvs_listen awvs<br />
docker cp /tmp/awvs awvs:/tmp/awvs 2&gt;/dev/null<br />
docker exec awvs bash -c "AWVS_DEBUG=${AWVS_DEBUG} bash &lt;(curl -sLk https://www.fahai.org/aDisk/Awvs/check-tools.sh)"<br />
rm -rf /tmp/awvs &gt;/dev/null 2&gt;&amp;1<br />
docker restart awvs &gt;/dev/null 2&gt;&amp;1<br />

&nbsp; [✓]&nbsp; Chmod license_info.json Success!<br />
&nbsp; [✓]&nbsp; Chmod wa_data.dat Success!<br />
&nbsp; [✓]&nbsp; Chmod wvsc Success!<br />
&nbsp; [✓]&nbsp; Chown wa_data.dat Success!<br />
&nbsp; [✓]&nbsp; Move license_info.json Success!<br />
&nbsp; [✓]&nbsp; Move wa_data.dat Success!<br />
&nbsp; [✓]&nbsp; Move wvsc Success!<br />
&nbsp; [✓]&nbsp; Add HOSTS.1 Success!<br />
&nbsp; [✓]&nbsp; Add HOSTS.2 Success!<br />
&nbsp; [✓]&nbsp; Add HOSTS.3 Success!<br />
&nbsp; [✓]&nbsp; Clean Success!<br />
<br />
<br />
