# optimize-note
#
<font size=5>前端性能优化学习笔记</font>
###
@author shima_lee
###

##
RAIL量化性能测量模型
###
```
| R: Response | A: Animation | I: Idle | L: Load |
```
###
##

##
RAIL评估标准
###
```
1、响应：处理事件应在50ms内完成
2、动画：尽量保持在60fps
3、空闲：尽可能增加空闲时间
4、加载：在5s内完成内容加载并可以交互
```
###
##

##
性能测试工具
###
```
1、Chrome DevTools
2、LightHouse
3、WebPageTest
```
###
##

##
WebPageTest使用笔记

###
```
1、waterfall chart
2、first view
3、repeat view
```
如何本地部署
```
install docker

docker pull webpagetest/server

docker pull webpagetest/agent

docker run -d -p 4000:80 webpagetest/server

docker run -d -p 40001:80 --network="host" -e "SERVER_URL=http://localhost:4000/work/" -e "LOCATION=Test" webpagetest/agent

```
###

###
mac 需制作自定义镜像
```
// wpt-server

mkdir wpt-mac-server

cd wpt-mac-server


vim Dockerfile
-----------------

FROM webpagetest/server
ADD locations.ini /var/www/html/settings/

-----------------


vim locations.ini
-----------------

[locations]
1=Test_loc
[Test_loc]
1=Test
label=Test Location
group=Desktop
[Test]
browser=Chrome,Firefox
label="Test Location"
connectivity=LAN

-----------------

docker build -t wpt-mac-server .


// wpt-agent

mkdir wpt-mac-agent

cd wpt-mac-agent


vim Dockerfile
-----------------

FROM webpagetest/agent
ADD script.sh/
ENTRYPOINT /script.sh

-----------------


vim script.sh
-----------------

 # !/bin/bash
   
set -e if [ -z "$SERVER_URL" ]; then echo >&2 'SERVER_URL not set' exit 1 fi if [ -z "$LOCATION" ]; then echo >&2 '
LOCATION not set' exit 1 fi EXTRA_ARGS=""
if [ -n "$NAME" ]; then EXTRA_ARGS="$EXTRA_ARGS --name $NAME"
fi python /wptagent/wptagent.py --server $SERVER_URL --location $LOCATION $EXTRA_ARGS --xvfb --dockerized -vvvvv
--shaper none

-----------------

chmod u+x script.sh

docker build -t wpt-mac-agent .

```
###
##

##
LightHouse使用笔记
###
```
npm i -g lighthouse
lighthouse http://www.baidu.com
// 测试完成后将地址copy进浏览器即可查看
```
###
##

#



