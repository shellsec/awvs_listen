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
