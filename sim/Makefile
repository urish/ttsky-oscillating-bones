MACRO_NAME=tt_um_oscillating_bones
MACRO_SPICE_FILE=$(MACRO_NAME).spice

ifndef PDK_ROOT
$(error PDK_ROOT is not set.  Please set PDK_ROOT to the root directory of the PDK (e.g. ~/.volare))
endif

sim: $(MACRO_SPICE_FILE) dut_inst.spice
	echo ".lib '$(PDK_ROOT)/sky130A/libs.tech/ngspice/sky130.lib.spice' tt" > pdk_lib.spice
	ngspice ./testbench.spice
.PHONY: sim

# magic's port order is not stable across tool versions, so build the instance
# line from the extracted .subckt header rather than hard-coding pin positions.
dut_inst.spice: $(MACRO_SPICE_FILE)
	python3 gen_dut_inst.py $< $@

$(MACRO_SPICE_FILE): ../mag/$(MACRO_NAME).mag
	magic -noconsole -dnull extract_spice_sim.tcl $(MACRO_NAME) $< $@

clean:
	rm -f $(MACRO_SPICE_FILE) pdk_lib.spice dut_inst.spice
.PHONY: clean

