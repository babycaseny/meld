# Meld for OS X

This README should help you build Meld for OS X. 

### Preparing JHBuild Environment

JHBuild is the build system that we will be using to build Meld. This step should really be done once and further builds should not require updating the build environment unless there has been some updates to the libraries that you'd like to do.

### Never Root

Through this tutorial, you should *never* be root. If at any point in time a script or command required access to your system - or to python site-packages on your system, then something is wrong. In which case, you should ensure that this command is running under jhbuild shell or from the ~/gtk/inst path. 

---

#### Initial Phase ####

######1. Download the setup script
```bash
cd ~
curl -O https://git.gnome.org/browse/gtk-osx/plain/gtk-osx-build-setup.sh
```

######2. Run the setup script
```bash
sh gtk-osx-build-setup.sh
~/.local/bin/jhbuild shell
```

######3. Build python
```bash
jhbuild build python
```

######4. Prepare paths and build the bootstrap
```bash
alias jhbuild="PATH=gtk-prefix/bin:$PATH jhbuild"
jhbuild bootstrap
```

######5. Checkout meld and start the initial phase
```bash
git clone https://github.com/yousseb/meld.git
cd meld
git checkout meld-1-8
cd osx/
ln -sf $PWD/jhbuildrc-custom ~/.jhbuildrc-custom
cd ..
jhbuild
```

######6. Fix the gtksourceview issues
```bash
ln -sf ~/gtk/inst/lib/pkgconfig/gtk-mac-integration-gtk2.pc ~/gtk/inst/lib/pkgconfig/gtk-mac-integration.pc
```
```bash
cp ~/Source/gtk/gtksourceview-2.10.5/tests/test-completion.c  ~/Source/gtk/gtksourceview-2.10.5/tests/test-widget.c
```
Edit: `~/Source/gtk/gtksourceview-2.10.5/gtksourceview/gtksourceview-i18n.c` and comment out: 
```
//if (quartz_application_get_bundle_id () != NULL)
//{
//    locale_dir = g_build_filename (quartz_application_get_resource_path (), "share", "locale", NULL);
//}
//else
```

######7. Resume the build
```
jhbuild
```

######8. Build extra dependencies
```bash
jhbuild -m osx/meld.modules build meld-python-deps
easy_install py2app
```

######9. You're now ready to build Meld. 
This is the command that you will be running over an over if you plan to customize the build. Most of the customization would actually be in thie build_app.sh file itself.
```bash
bash osx/build_app.sh
```
