SLOTS ?= $(find /sys/devices/ -name slots)
export
default: uninstall install
uninstall: bbb_switched_led_test.uninstall
install: bbb_switched_led_test.install
%.uninstall:
	SLOT=$$(cat $(SLOTS) | awk -F: '$$2 ~ /$*/ {print $$1}'); \
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
