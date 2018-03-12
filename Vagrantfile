# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  massive_start = false
  packages = "vim zsh git tmux"

  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "synced", "/vagrant", create: true

  if massive_start
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
  end

  # dotfiles
  %w(zshrc vimrc gitconfig tmux.conf).each do |name|
    config.vm.provision "dotfile #{name}", type: "file",
      source: "~/.#{name}", destination: ".#{name}"
  end

  # vim colors and main plugins
  config.vm.provision "vim colors", type: "file",
    source: "~/.vim/colors", destination: ".vim/"
  config.vm.provision "vim plugin manager plug", type: "file",
    source: "~/.vim/autoload/plug.vim", destination: ".vim/autoload/plug.vim"
  config.vm.provision "vim plugin nerdcommenter", type: "file",
    source: "~/.vim/plugged/nerdcommenter/plugin", destination: "$HOME/.vim/plugged/nerdcommenter/"

  # virtual provision (should be redefined in each machine)
  config.vm.provision "install packages",
    type: "shell",
    preserve_order: true,
    inline: <<~EOF
      echo Redefine this provision >&2
      exit 1
    EOF

  # preserve_order need to install zsh before making it default shell
  config.vm.provision "change shell to zsh",
    type: "shell",
    preserve_order: true,
    inline: <<~EOF
      [ $SHELL = /bin/zsh ] || [ -x /bin/zsh ] && usermod -s /bin/zsh vagrant
    EOF

  config.vm.define "archlinux", autostart: massive_start do |archlinux|
    archlinux.vm.box = "archlinux/archlinux"
    archlinux.vm.provision "install packages",
      type: "shell",
      preserve_order: true,
      inline: <<~EOF
        pacman -Sy --noconfirm --needed #{packages}
      EOF
  end

  config.vm.define "ubuntu", autostart: massive_start do |ubuntu|
    ubuntu.vm.box = "ubuntu/artful64"
    ubuntu.vm.hostname = "ubuntu"
    ubuntu.vm.provision "install packages",
      type: "shell",
      preserve_order: true,
      inline: <<~EOF
        apt-get update
        apt-get install -y #{packages}
      EOF
  end

  config.vm.define "debian", autostart: massive_start do |debian|
    debian.vm.box = "debian/stretch64"
    debian.vm.hostname = "debian"
    debian.vm.provision "install packages",
      type: "shell",
      preserve_order: true,
      inline: <<~EOF
        apt-get update
        apt-get install -y #{packages}
      EOF
  end

  config.vm.define "fedora", autostart: massive_start do |fedora|
    fedora.vm.box = "fedora/27-cloud-base"
    fedora.vm.hostname = "fedora"
    fedora.vm.provision "install packages",
      type: "shell",
      preserve_order: true,
      inline: <<~EOF
        dnf install -y #{packages}
      EOF
  end

  config.vm.define "opensuse", autostart: massive_start do |opensuse|
    opensuse.vm.box = "opensuse/openSUSE-Tumbleweed-x86_64"
    opensuse.vm.hostname = "opensuse"
    opensuse.vm.provision "install packages",
      type: "shell",
      preserve_order: true,
      inline: <<~EOF
        zypper install -y --force-resolution #{packages}
      EOF
  end
end
