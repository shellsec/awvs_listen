下面下面下载说明：https://www.fahai.org
本博客所发布的Docker_Awvs23.x一键安装脚本[支持版本更新]仅限用于学习和研究目的；
感谢：XrSec
当前软件版本：23.6.230628115
【注1：由于服务器连通性原因，可能导致激活失败问题，可尝试重新安装！】
【注2：推荐安装系统 Centos7 / Kali / Ubuntu 】

破解方法：
1、下载 check.sh 文件至服务器内执行或直接使用下面命令

bash <(curl -sLk https://www.fahai.org/aDisk/Awvs/check.sh) xrsec/awvs

或

bash <(curl -sLk https://www.fahai.org/aDisk/Awvs/check.sh) registry.cn-hangzhou.aliyuncs.com/xrsec/awvs
bash <(curl -sLk https://www.fahai.org/aDisk/Awvs/check.sh) registry.cn-hangzhou.aliyuncs.com/xrsec/awvs

报错了
  [✓]  Start Install
  [✓]  The Container awvs Was Deleted Success!
  [✓]  Docker Pull xrsec/awvs Success!
  [✓]  Create AWVS container Success!
  [✓]  The current version is the latest version
  [✓]  Download awvs_listen.zip Success!
  [✗]  Unzip awvs_listen.zip failed

假设已经执行到这一步！执行下面即可
docker exec awvs bash -c "cat /awvs/LAST_VERSION | sed 's/ //g' 2>/dev/null"
mkdir /tmp/awvs >/dev/null 2>&1
rm -rf /tmp/awvs_listen.zip
cd /tmp/
git clone https://github.com/shellsec/awvs_listen.git
mv awvs_listen awvs
docker cp /tmp/awvs awvs:/tmp/awvs 2>/dev/null
docker exec awvs bash -c "AWVS_DEBUG=${AWVS_DEBUG} bash <(curl -sLk https://www.fahai.org/aDisk/Awvs/check-tools.sh)"
rm -rf /tmp/awvs >/dev/null 2>&1
docker restart awvs >/dev/null 2>&1

  [✓]  Chmod license_info.json Success!
  [✓]  Chmod wa_data.dat Success!
  [✓]  Chmod wvsc Success!
  [✓]  Chown wa_data.dat Success!
  [✓]  Move license_info.json Success!
  [✓]  Move wa_data.dat Success!
  [✓]  Move wvsc Success!
  [✓]  Add HOSTS.1 Success!
  [✓]  Add HOSTS.2 Success!
  [✓]  Add HOSTS.3 Success!
  [✓]  Clean Success!




