# Windows ansible

quick windows ansible POC


## Setup MacOS dev machine

    # install remote desktop
    brew install microsoft-remote-desktop

    # create a Windows 10 VM
    brew install vagrant
    vagrant init gusztavvargadr/windows-10
    vagrant up

    # view the usename, password, ip, and port of vagrant VM
    vagrant winrm-config

    # create a snapshot so we can easily revert
    vagrant snapshot save initialImage

    # get a fresh python
    brew install pyenv
    pyenv install 3.9.0
    pyenv global 3.9.0
    echo 'eval "$(pyenv init -)"' >> ~/.zshrc
    echo 'eval "$(pyenv init --path)"' >> ~/.zshrc
    source ~/.zshrc
    python --version
    pip --version
    
    # install ansible
    pip install -r ./requirements.txt
    
    # install the windows modules
    ansible-galaxy collection install ansible.windows
    ansible-galaxy collection install community.windows

    # stop the vagrant vm
    vagrant halt


## Run the playbook

    # workaround for macos ansible segfault
    # https://www.whatan00b.com/posts/debugging-a-segfault-from-ansible/
    # https://github.com/ansible/ansible/issues/32554
    export no_proxy=’*’

    # test ping vagrant host
    ansible -i inventory vagrant -m win_ping

    # run playbook
    ansible-playbook -i inventory -l vagrant playbook.yaml

    # validate configs by logging into new host
    open "/Applications/Microsoft Remote Desktop.app"

    # reset the vagrant vm
    vagrant snapshot restore initialImage

    # stop the vagrant vm
    vagrant halt


## Docs

- https://docs.ansible.com/ansible/latest/collections/index_module.html#ansible-windows
- https://docs.ansible.com/ansible/latest/collections/index_module.html#community-windows
- https://github.com/magenta-aps/vagrant-ansible-example-windows
- https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html

