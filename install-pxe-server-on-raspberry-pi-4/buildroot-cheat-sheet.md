# Buildroot cheat sheet

```
make distclean
make ghatanothoa_defconfig
make xconfig # if any additional changes
make savedefconfig # save these additional changes
make linux-xconfig # if any additional changes
# save linux config
cp output/build/linux-5.12.10/.config \
    board/<brand>/<board>/linux.config 
make
```
