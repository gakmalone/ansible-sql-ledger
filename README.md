Ansible Role for SQL-Ledger
===========================

This role allows you to install SQL-Ledger on Debian Jessie.

Clone it to your Ansible roles directory:

    git clone https://github.com/Tekki/ansible-sql-ledger.git path_to_ansible/roles/sql-ledger

Prerequisites
-------------

The machine from which the playbook is being run needs to have Ansible 2.0
or higher installed. For detailed information how to obtain current packages for your
distribution of choice have a look at the
[Ansible documentation](https://docs.ansible.com/ansible/intro_installation.html).

The target machine needs SSH access and Python and sudo installed. As standard Debian
comes without sudo, you should install it and update your host configuration:

    ansible_become: true
    ansible_become_pass: "{{ vault_sudo_pwd }}"

Role Variables
--------------

The following variables can be passed to this role:

| Variable Name | Default Value | Description |
| ------------- | ------------- | ----------- |
| sl_admin_pwd | *undefined* | password for admin.pl |
| sl_dvipdf | 0 | use dvipdf instead of pdflatex |
| sl_git_branch | full | branch that will be checked out |
| sl_git_source | https://github.com/Tekki/sql-ledger.git | URL of the Git repository |
| sl_httpd_path | /var/www/sql-ledger | local path of the installation |
| sl_httpd_url | sql-ledger | browser URL on the server |
| sl_latex | 1 | install and use LaTeX |
| sl_login_language | | language of the login screen |
| sl_pdftk | 1 | use pdftk to combine PDFs |
| sl_postgres_user | sql-ledger | user name to connect to PostgreSQL |
| sl_sendmail | "\| /usr/sbin/sendmail -f <%from%> -t" | pipe to sendmail |
| sl_xelatex | 0 | use xelatex instead of pdflatex |
| texlive_lang | german | language of texlive that will be installed |

Please notice that this role doesn't install any mail transport agent.

Example Playbook
----------------

To install with the default settings and language German/Switzerland (chd_utf) on the login screen, use the following playbook:

    - hosts: sql-ledger-servers
      roles:
         - { role: sql-ledger, sl_login_language: chd_utf }

If you want to set the password for admin.pl to '12345678' (probably not a good idea), write:

    - hosts: sql-ledger-servers
      roles:
         - { role: sql-ledger, sl_admin_pwd: 12345678 }

Use the [Vault](http://docs.ansible.com/ansible/playbooks_vault.html) to protect your
passwords!

To make the original version from DWS available under /sl-dws, write:

    - hosts: sql-ledger-servers
      roles:
        - { role: sql-ledger, sl_httpd_path: /var/www/sl-dws, sl_httpd_url: sl-dws, sl_git_branch: master }

It is possible to install the role multiple times under different URLs on the same server.

If your hosts are defined in *sql-ledger.hosts* in this way:

    [sql-ledger-servers]
    sl-server-[01:05]

and the name of the playbook is *sql-ledger.yml*, you can start the installation with:

    ansible-playbook -i sql-ledger.hosts sql-ledger.yml

Be aware that the installation of texlive needs a lot of time.

Update
------

To update the installation, simply run the playbook again.

License
-------

[GPL3](LICENSE)
