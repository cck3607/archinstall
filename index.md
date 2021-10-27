## Start of Install 

The first step is downloading the arch iso. It is very important that you connect this iso via the setting and cd/disk image. Once connected the first thing you should do is check you internet connect. The first iso I downloaded did not allow me to connect to the internet after a few hours of trying to fix it I finally downloaded a different iso that connected to the internet right away.

### Once inside the VM

After gaining access to the VM with your iso follow these steps.

```markdown


# Confirming internet 
ip addr show
ping google.com
## Set The Console Keyborad Layout
ls /usr/share/kbd/keymaps/**/*.map.gz 
## Verify The Boot Mode

ls /sys/firmware/efi/efivars

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/cck3607/archinstall/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
