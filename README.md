# gentoo-generate-conf.d-modules

```
Usage: generate-conf.d-modules [--load-all-modules]
```

`generate-conf.d-modules` will examine the currently loaded modules and build a
file suitable for deployment as /etc/conf.d/modules based on the results. Where
modules support additional arguments, these are automatically documented if
possible.  In addition, all modules are categorised and categories are squashed
if possible where there is a single leaf-node.  A sample segment might read:

```

# drivers/block:
#
modules="${modules} loop"
#modules="${modules} cryptoloop"
#modules="${modules} zram"
#module_loop_args="max_loop=(int),max_part=(int)"
#module_zram_args="num_devices=(uint)"

```

... which shows that there are three modules in the `drivers/block` category,
one of which (`loop`) should be auto-loaded.  Two modules (`loop` and `zram`)
take optional additional arguments which may be updated and uncommented if
required.

If the `--load-all-modules` option is specified and the script is run as `root`
then every available module for the current kernel will be loaded and a more
complete configuration generated.  Please note that this is theoretically
potentially dangerous if a present but unloaded module clashes with existing
hardware drivers or similar.

Additionally, modules which are present only as dependencies of other modules
are gathered in a separate section.

To use:

```
sudo generate-conf.d-modules --load-all-modules > sudo tee /etc/conf.d/modules.new
```
