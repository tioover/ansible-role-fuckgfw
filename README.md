# ansible-role-fuckgfw

[Ansible](https://github.com/ansible/ansible) 自动配置翻墙，需要 docker。

playbook 使用例子与变量说明

`fuckgfw.yml`

~~~yaml
- hosts: fuckgfw # 你需要翻墙的主机 group
  become: yes
  become_user: dockeradmin # 你的有 docker 权限的用户，可以是 root
  var:
    vpn_user: "{{ lookup('env', 'USER') }}" # 默认用户名为 $USER
    fuckgfw_directory: "{{ ansible_env.HOME }}/fuckgfw" # 配置文件等东西的目录，默认为 $HOME/fuckgfw

    # ⚠️注意！！！私有信息，必须更改！！！注意⚠️
    fuckgfw_password: W9C22ziz3 # 所有需要密码的翻墙服务的密码
    shadowsocks_port: 8959 # shadowsocks 端口
    vmess_port: 9376 # VMess 协议端口
    vmess_uuid: b529638c-d8cb-4c35-ba1f-5f2bfd1d8d00 # VMess 协议 UUID
    ipsec_psk: E3rm6N99g # IPSEC 共享密钥
    firewalld: true # firewalld 防火墙自动配置，如果没有防火墙，或者不用 firewalld 请设为 false
  roles:
    - fuckgfw
~~~

hosts 是你的主机列表文件，详细请阅读 ansible 文档
~~~shell
ansible-playbook -i hosts docker-setup.yml
~~~

防火墙自动配置仅支持 `firewalld`，ufw 等请自己改文件

更成熟的项目： [streisand](https://github.com/jlund/streisand) *Ubuntu Only*s

# TODO

* Tor bridge