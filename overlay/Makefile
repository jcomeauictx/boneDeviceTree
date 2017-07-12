CONFIG ?= bbb_switched_led_test
SLOTS ?= $(find /sys/devices/ -name slots)
DRYRUN ?= --dry-run
export
default: uninstall install
uninstall: $(CONFIG).uninstall
install: $(CONFIG).install
%.uninstall:
	SLOT=$$(echo $$(cat $(SLOTS) | awk -F: '$$NF ~ /$*/ {print $$1}')); \
	echo SLOT=$$SLOT >&2; \
	[ "$$SLOT" ] && echo -$$SLOT > $(SLOTS) || true
%.install: %-00A0.dtbo
	cp -f $< /lib/firmware
	echo $* > $(SLOTS)
%-00A0.dtbo: %.dts
	dtc -O dtb -o $@ -b 0 -@ $<
# decompile rules follow
# comment them out if you leave .dtbo files in this directory
%.dts: %.dtbo
	dtc -I dtb -O dts $< > $@
%.dts: %-00A0.dtbo
	dtc -I dtb -O dts $< > $@
upload: # to BBB, `make DRYRUN= upload` when you're sure
	rsync -avuz -e "ssh -o StrictHostKeyChecking=no \
	 -o UserKnownHostsFile=/dev/null" $(DRYRUN) . root@192.168.7.2:$(PWD)/
