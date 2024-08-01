up：20240730&nbsp; awvs.sh 一键更新可用版本，自己改密码呗<br />
证书有效期<br />
Licensed<br />
Expires on Sep 19, 2123<br />
Licensed targets<br />
100000<br />
<br />
#!/bin/bash<br />
# 停止并删除 awvs 相关的容器和镜像 # 使用 || true 避免因权限问题导致的脚本中断<br />
docker stop $(docker ps -a | grep "awvs" | awk '{print $1}')<br />
chattr -i -R /var/lib/docker/overlay2/* || true &nbsp;<br />
docker rm $(docker ps -a | grep "awvs" | awk '{print $1}')<br />
docker rmi -f $(docker images | grep "awvs" | awk '{print $3}')<br />
docker rmi --no-prune $(docker images | grep "awvs" | awk '{print $3}')<br />
<br />
# 启动 awvs 容器<br />
docker run -it -d --name awvs -p 3443:3443 --restart=always xrsec/awvs

# 下面最好一行一行执行<br />
# 下载 awvs_listen.zip 并复制到容器中<br />
rm -rf awvs_listen.zip*<br />
<br />
curl -s -o awvs_listen.zip https://gh-proxy.com/https://raw.githubusercontent.com/shellsec/awvs_listen/master/awvs_listen02.zip || { echo "Failed to download awvs_listen.zip"; exit 1; }<br />
<br />
#wget https://raw.githubusercontent.com/shellsec/awvs_listen/master/awvs_listen02.zip<br />
#mv awvs_listen02.zip awvs_listen.zip<br />
<br />
docker cp awvs_listen.zip awvs:/awvs/<br />
<br />
# 解压文件并修改权限<br />
docker exec -i awvs /bin/bash -c 'unzip -o /awvs/awvs_listen.zip -d /home/acunetix/.acunetix/data/license/'<br />
docker exec -i awvs /bin/bash -c 'chmod 444 /home/acunetix/.acunetix/data/license/{license_info.json,wa_data.dat}'<br />
docker exec -i awvs /bin/bash -c 'chown acunetix:acunetix /home/acunetix/.acunetix/data/license/{license_info.json,wa_data.dat}'<br />
<br />
# 移动 wvsc 文件并修改权限<br />
<br />
docker exec -i awvs /bin/bash -c 'mv /home/acunetix/.acunetix/data/license/wvsc /home/acunetix/.acunetix/v_*/scanner/'<br />
docker exec -i awvs /bin/bash -c 'chmod 777 /home/acunetix/.acunetix/v_*/scanner/wvsc'<br />
docker exec -i awvs /bin/bash -c 'chown acunetix:acunetix /home/acunetix/.acunetix/v_*/scanner/wvsc'<br />
<br />
<br />
# 删除 zip 文件和修改 hosts 文件<br />
docker exec -i awvs /bin/bash -c 'rm -rf /awvs/awvs_listen.zip*'<br />
<br />
docker exec awvs bash -c "echo '127.0.0.1 updates.acunetix.com' &gt;/awvs/.hosts"<br />
docker exec awvs bash -c "echo '127.0.0.1 erp.acunetix.com' &gt;&gt;/awvs/.hosts"<br />
docker exec awvs bash -c "echo '127.0.0.1 telemetry.invicti.com' &gt;&gt;/awvs/.hosts"<br />
docker exec awvs bash -c 'cat /awvs/.hosts'<br />
<br />
echo '127.0.0.1 erp.acunetix.com' | tee -a /etc/hosts<br />
echo '192.178.49.174 telemetry.invicti.com' | tee -a /etc/hosts<br />
echo '::1 erp.acunetix.com' | tee -a /etc/hosts<br />
echo '2607:f8b0:402a:80a::200e telemetry.invicti.com' | tee -a /etc/hosts<br />
<br />
<br />
# 重启 awvs 容器并删除 zip 文件<br />
docker restart awvs<br />
rm -rf awvs_listen.zip*<br />
<br />
<br />
<br />
<br />
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
