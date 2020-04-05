自建docker 


（ps.建议本地编译后直接通过docker生成image）

（ps.GITHUB ACTION云编译流程已完成测试，过几天更新）

预计4月6日后增加图文教程


全是经验之谈，算不上专业。





1、编译固件



先编译lede 固件(感谢大佬https://github.com/coolsnowwolf/lede)



无论云编译/本地编译，勾选 Target Image>***Root filesystem archives***>tar.gz



其他插件自己按需选取吧！





2、新建Dockerfiles





###############本地环境###################



前提：安装了docker



mkdir dockerimg #文件夹名随便



cd dockerimg



vim Dockerfile  #必须叫Dockerfile（据说是必须）



键入以下内容>>



    FROM scratch



    ADD openwrt-bcm27xx-bcm2709-rpi-2-rootfs.tar.gz /



    USER root



    CMD /sbin/init



将前一步编译的openwrt-bcm27xx-bcm2709-rpi-2-rootfs.tar.gz文件拷贝至dockerimg文件夹下方



docker build -f ./Dockerfile -t name:tagname .   (仅本地使用的话，name取一个简单的比如rpi，tagname可以随意如当天日期
20200327。最后的 . 不能少）





###############Docker HUb自建镜像##########



！！！需要扶梯子！！！



本地编译用户tar.gz超过100MB的话，建议使用bitbucket。



在bitbucket中建立项目；



结构可以是：



docker-project                 （文件夹/项目）



        >Dockerfile



        >openwrt-bcm27xx-bcm2709-rpi-2-rootfs.tar.gz



Dockerfile内容>>



    FROM scratch



    LABEL maintainer="yourusername - https://bitbucket.org/yourusername"



    ADD openwrt-bcm27xx-bcm2709-rpi-2-rootfs.tar.gz /



    USER root



    CMD /sbin/init



在Dockerhub中链接你的bitbucket.org账户设置自动建立镜像即可。





3、运行-设置lede容器



最终在本地生成image的同学接下来的步骤可参考https://mlapp.cn/376.html



最终在docker.com上生成imang的同学可以pull到本地再参考上面的链接。



其中创建并使用容器的指令，



docker run --restart always --name openwrt -d --network macnet --privileged registry.cn-
shanghai.aliyuncs.com/suling/openwrt:latest /sbin/init



将registry.cn-shanghai.aliyuncs.com/suling/openwrt:latest改为image ID即可



image ID：docker images获取本地镜像



REPOSITORY********************************TAG******************IMAGE ID	************CREATED************SIZE



docker.io/muyeyifeng/rpi2-lede************latest****************0077368fb938********36 minutes ago******288 MB



这里的ID就是0077368fb938


docker run --restart always --name openwrt -d --network macnet --privileged 0077368fb938 /sbin/init





#######云编译直接生成DOCKER见DOCX文件说明###########
