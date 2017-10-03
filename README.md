# ansible-role-fuckgfw

[Ansible](https://github.com/ansible/ansible) Ubuntu 自动配置翻墙，需要 docker。

**安全性警告**： 配置依赖于 DockerHub 上的镜像：[ipsec-vpn-server](https://hub.docker.com/r/hwdsl2/ipsec-vpn-server/) [v2ray offical](https://hub.docker.com/r/v2ray/official/) 

如果不想深入了解的话，安装 ansible 以后

~~~
mkdir roles
git clone https://github.com/tioover/ansible-role-fuckgfw.git roles/fuckgfw
touch hosts
touch fuckgfw.yml
~~~

`fuckgfw.yml` 文件

~~~yaml
- hosts: fuckgfw # 你需要翻墙的主机 group
  become: yes
  become_user: root
  var:
    docker_admin: admin # 你的有 docker 权限的用户，可以是 root
    vpn_user: "{{ lookup('env', 'USER') }}" # 默认用户名为 $USER
    fuckgfw_directory: "/home/admin/fuckgfw" # 配置文件等东西的目录

    # ⚠️注意！！！私有信息，必须更改，随机化！！！注意⚠️
    fuckgfw_password: W9C22ziz3 # 所有需要密码的翻墙服务的密码
    shadowsocks_port: 8959 # shadowsocks 端口
    vmess_port: 9376 # VMess 协议端口
    vmess_uuid: b529638c-d8cb-4c35-ba1f-5f2bfd1d8d00 # VMess 协议 UUID，自己随机生成
    obfs4_port: 4948 # Tor obf4 端口
    tor_nickname: anonymous # Tor 网桥昵称
    ipsec_psk: E3rm6N99g # IPSEC 共享密钥
  roles:
    - fuckgfw
~~~
`hosts` 文件

~~~ini
[fuckgfw]
42.42.42.42 # 你要翻墙的主机，需要 root SSH Key 免密码登陆
233.233.233.233 # 可以有多个
~~~

然后执行

~~~shell
ansible-playbook -i hosts docker-setup.yml
~~~

更成熟的项目： [streisand](https://github.com/jlund/streisand)

# TODO

- [x] Tor bridge
- [ ] 多种协议，混淆流量
- [ ] 验证依赖的可信性，以免被偷偷替换

……发 issue 建议