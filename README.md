Requirements: debian/ubuntu vm

Steps:
- create admin user
- configure sshd
- install and configure firewall (ufw)
- install mtproxy

HowTo:

1. create vm (with debian/ubuntu distro)
2. clone repo

  ```bash
  git clone https://github.com/cimon-io/mtproxy-playbook.git
  cd mtproxy-playbook
  ```

3. download roles

  ```bash
  ansible-galaxy install -r requirements.yml
  ```

4. configure
  - set ansible_host options in `inventory/production/host_vars/mtproxy-01`
  - copy your public ssh key to `files/public_keys/your_name.pub`

    `# cp ~/.ssh/id_rsa.pub files/public_keys/$USER.pub`

  - edit vars/users.yml file: replace your_name with your users name

    `# sed -i "s/your_name/$USER/g" vars/users.yml`

  - add `mtproxy_mtproto_secret` variable to vars/proxy.yml

    `# echo "mtproxy_mtproto_secret: \"$(head -c 16 /dev/urandom | xxd -ps)\"" >> vars/proxy.yml`

5. run playbook

  > if python is not installed, run: ansible-playbook -i production prepare.yml -e ansible_user=root

  > ansible_user: root, if you use AWS EC2 with ubuntu - user wil be 'ubuntu'

  > specify --private-key=path-to-private-key if necessary

  `ansible-playbook -i production main.yml -e ansible_user=root`

6. register your proxy with @MTProxybot on Telegram
7. Add `mtproxy_proxy_tag` variable with your tag to vars/proxy.yml

  `# echo "mtproxy_proxy_tag: \"your_tag_here\"" >> vars/proxy.yml`

8. ... and simply run

  `ansible-playbook -i production main.yml -tags=mtproxy-configure`
