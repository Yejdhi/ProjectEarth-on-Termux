# ProjectEarth-on-Termux

## 目录

* [准备](#准备)
  * 安装Termux
  * 更新
  * 安装
  * 进入Ubuntu
  * 更新源和安装依赖
* [API](#API)
  * 创建文件夹
  * 下载和解压
  * 修改配置文件
  * 启动
* [Cloudburst](#Cloudburst)
  * 创建文件夹
  * 下载和解压
  * 修改配置文件
  * 启动
  


## 准备

1. 安装Termux

    [点击下载](https://github.com/termux/termux-app/releases/download/v0.118.0/termux-app_v0.118.0+github-debug_arm64-v8a.apk)

2. 更新

   ```shell copy
   pkg update -y
   ```

3. 安装

    > 安装PRootDistro和PRoot中的Ubuntu

    ```shell
    pkg install proot-distro -y
    ```
    ```shell
    proot-distro install ubuntu
    ```

> [!Tip]
> proot-distro install ubuntu
> 可以简写为
> `pd i ubuntu`


4. 进入Ubuntu
    ```shell
    proot-distro login ubuntu
    ```
> [!Tip]
> 可以简写为
> `pd sh ubuntu`


5. 更新源和安装依赖

    ```shell
   apt-get update -y && apt-get install -y libgssapi-krb5-2 openjdk-8-jre-headless
    ```
    > 手动安装libssl1.1
    ```shell
    wget http://ports.ubuntu.com/ubuntu-ports/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_arm64.deb
    ```
    ```shell
    dpkg -i libssl1.1_1.1.1f-1ubuntu2_arm64.deb
    ```



---

## API

1. 准备工作

    > 创建文件夹

    ```shell
    cd && mkdir API
    ```

    > 进入文件夹
    ```shell
    cd API
    ```

2. 下载和解压

    > 下载API

    ```shell
    wget https://github.com/HazukiY1/Api/releases/download/arm64/linux-arm64.zip
    ```

    > 解压

    ```shell
    unzip linux-arm64.zip
    ```

    > 下载资源包

    ```shell
    wget -P data/resourcepacks https://github.com/HazukiY1/Api/releases/download/arm64/vanilla.zip
    ```

3. 修改配置文件

    > 修改apiconfig.json

    ```shell
    vi data/config/apiconfig.json
    ```

> [!Tip]
> 键盘上按`i`出现`--INSERT--`进入编辑模式  
> 按下`ESC`退出编辑模式  
> 输入`:wq`保存文件  
> 输入`:q!`强制不保存退出

> [!Warning]
> 仅供参考，请不要直接全部复制

```json
{
    "baseServerIP": "此处修改为 http:// + 你的IPv4 + 端口",
    
    "省略..."

    "tappableSpawnRadius": 0.001,
    "multiplayerAuthKeys": {
        "这里只填IPv4": "/g1xCS33QYGC+F2s016WXaQWT8ICnzJvdqcVltNtWljrkCyjd5Ut4tvy2d/IgNga0uniZxv/t0hELdZmvx+cdA=="
    }
}
```

> `baseServerIP`示例：  
> `http://192.168.10.1:8089`  
> 
> 端口需大于`1024`  
> 
> `IPv4`示例：  
> `"192.168.10.1": ""`  
> 注意`0.001`后需要加逗号`,`


> 修改端口

```shell
vi appsettings.json
```

```json
{
  "Kestrel": {
    "EndPoints": {
      "Http": {
        "Url": "http://*: 端口"
      }
    }
  },

  "省略..."

}
```

> `端口`改为和上一步`baseServerIP`中一样的端口(>=1024)


4. 启动

    > 授予权限
    ```shell
    chmod +x ./ProjcetEarthServerAPI
    ```

    > 添加变量
    >> 原因是API启动时有概率报  
    >> `Couldn't find a valid ICU package installed on the system`
    ```shell
    echo "export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1" >> .bashrc
    ```

    > 启动
    ```shell
    ./ProjectEarthServerAPI
    ```

> [!Tip]
> `Ctrl` + `C` 关闭服务器

---



## Cloudburst

1. 准备工作

    > 创建文件夹

    ```shell
    cd && mkdir Cloudbusrt
    ```

    > 进入文件夹

    ```shell
    cd Cloudbusrt
    ```

2. 下载和解压

    > 下载
    ```shell
    wget https://github.com/ENDERMANYK/API/releases/download/Patch-4/Cloudburst.zip
    ```

    > 解压
    ```shell
    unzip Cloudburst.zip
    ```  

3. 修改配置文件

```shell
vi cloudburst.yml
```

```yml
settings:               
  earth-api: "和API相同的 IPv4 : 端口 + /1/api"         
  enable-secure-api-connections: false
  # Multi-language setting
  language: "zh_CN"
  # 省略...
```

> `earth-api`示例  
> `"192.168.10.1:8089/1/api"`

4. 启动

    ```shell
    java -jar Cloudburst.jar
    ```

> [!Tip]
> `stop` 命令关闭服务器

---

## 后续
> [!Warning]
> 完成了上面所有的步骤后，想再次启动服务器时

1. 打开`Termux`输入

    ```shell
    pd sh ubuntu
    ```

2. 进入API文件夹并启动

    ```shell
    cd API && nohup ./ProjectEarthServerAPI &
    ```

3. 进入Cloudburst文件夹并启动

    ```shell
    cd ~/Cloudburst
    ```

    ```shell
    java -jar Cloudburst.jar
    ```

    或者

    ```shell
    nohup java -jar Cloudburst.jar &
    ```

二选一

> 区别是
> nohup 开头的是在后台运行

> [!Tip]
> `fg`命令可以回到前台运行