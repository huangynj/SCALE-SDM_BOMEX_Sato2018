################################################################################
#
# Makefile for main program
#
################################################################################

TOPDIR      = $(abspath ../..)
BUILD_DIR   = ./.libs
SYSDEP_DIR  = ../../sysdep

include $(SYSDEP_DIR)/Makedef.$(SCALE_SYS)
include $(TOPDIR)/Mkinclude

VPATH = $(BUILD_DIR): \
        include:      \
        atmos:        \
        land:         \
        ocean:        \
        cpl:          \
        user:         \
        preprocess:

VERSION = $(shell git rev-parse --short HEAD 2> /dev/null)
ifeq ($(VERSION),)
  VERSION  = $(shell cat VERSION)
else
  VERSION := $(VERSION)
endif

DEPENDLIB = $(LIBDIR)/libscale.a $(LIBDIR)/libgtool.a $(LIBDIR)/libdcutils.a

BINNAME    = scale-les
INITNAME   = scale-les_init
PPNAME     = scale-les_pp

PRJS = $(BINDIR)/$(BINNAME) \
       $(BINDIR)/$(INITNAME) \
       $(BINDIR)/$(PPNAME)

MODS =	\
	mod_atmos_vars.o	\
	mod_atmos_vars_sf.o	\
	mod_atmos_dyn_driver.o	\
	mod_atmos_phy_mp_driver.o	\
	mod_atmos_phy_sf_driver.o	\
	mod_atmos_phy_rd_driver.o \
	mod_atmos_phy_tb_driver.o	\
	mod_atmos_phy_ae_driver.o \
	mod_atmos.o	\
	mod_land_vars.o	\
	mod_land_phy_bucket.o	\
	mod_land.o	\
	mod_ocean_vars.o	\
	mod_ocean_phy_slab.o	\
	mod_ocean.o	\
	mod_cpl_vars.o	\
	mod_cpl_atmos_land_driver.o \
	mod_cpl_atmos_ocean_driver.o \
	mod_cpl.o \
	mod_mktopo.o	\
	mod_mkinit.o	\
	mod_cnvboundary.o	\
	mod_user.o



all: 
	$(MAKE) makedir 
	$(MAKE) makescalelib 
	$(MAKE) makebin

makedir:
	mkdir -p $(BINDIR)
	mkdir -p $(BUILD_DIR)

makescalelib:
	$(MAKE) -C $(SCALELIBDIR)/src depend
	$(MAKE) -C $(SCALELIBDIR)/src

makebin: $(PRJS)

ifeq ($(SDM),-D_SDM)
$(BINDIR)/$(BINNAME)  : $(BUILD_DIR)/$(BINNAME).o  $(patsubst %,$(BUILD_DIR)/%,$(MODS)) $(LIBDIR)/libscale.a
	$(FC) $(LDFLAGS) -o $@ $^ $(DEPENDLIB) $(NETCDF_LIBS) $(CONTRIB_LIBS) $(LAPACK_LIBS) $(PAPI_LIBS)

$(BINDIR)/$(INITNAME) : $(BUILD_DIR)/$(INITNAME).o $(patsubst %,$(BUILD_DIR)/%,$(MODS)) $(LIBDIR)/libscale.a
	$(FC) $(LDFLAGS) -o $@ $^ $(DEPENDLIB) $(NETCDF_LIBS) $(CONTRIB_LIBS) $(NETCDF_LIBS) $(LAPACK_LIBS) $(PAPI_LIBS)

$(BINDIR)/$(PPNAME)   : $(BUILD_DIR)/$(PPNAME).o   $(patsubst %,$(BUILD_DIR)/%,$(MODS)) $(LIBDIR)/libscale.a
	$(FC) $(LDFLAGS) -o $@ $^ $(DEPENDLIB) $(NETCDF_LIBS) $(CONTRIB_LIBS) $(NETCDF_LIBS) $(LAPACK_LIBS) $(PAPI_LIBS)
else
$(BINDIR)/$(BINNAME)  : $(BUILD_DIR)/$(BINNAME).o  $(patsubst %,$(BUILD_DIR)/%,$(MODS)) $(LIBDIR)/libscale.a
	$(FC) $(LDFLAGS) -o $@ $^ $(DEPENDLIB) $(NETCDF_LIBS) $(LAPACK_LIBS) $(PAPI_LIBS)

$(BINDIR)/$(INITNAME) : $(BUILD_DIR)/$(INITNAME).o $(patsubst %,$(BUILD_DIR)/%,$(MODS)) $(LIBDIR)/libscale.a
	$(FC) $(LDFLAGS) -o $@ $^ $(DEPENDLIB) $(NETCDF_LIBS) $(NETCDF_LIBS) $(LAPACK_LIBS) $(PAPI_LIBS)

$(BINDIR)/$(PPNAME)   : $(BUILD_DIR)/$(PPNAME).o   $(patsubst %,$(BUILD_DIR)/%,$(MODS)) $(LIBDIR)/libscale.a
	$(FC) $(LDFLAGS) -o $@ $^ $(DEPENDLIB) $(NETCDF_LIBS) $(NETCDF_LIBS) $(LAPACK_LIBS) $(PAPI_LIBS)
endif

$(BUILD_DIR)/$(BINNAME).o  : $(BINNAME).f90  $(DEPENDLIB) $(patsubst %,$(BUILD_DIR)/%,$(MODS))
$(BUILD_DIR)/$(INITNAME).o : $(INITNAME).f90 $(DEPENDLIB) $(patsubst %,$(BUILD_DIR)/%,$(MODS))
$(BUILD_DIR)/$(PPNAME).o   : $(PPNAME).f90   $(DEPENDLIB) $(patsubst %,$(BUILD_DIR)/%,$(MODS))



$(BUILD_DIR)/mod_atmos_dyn_driver.o       : mod_atmos_dyn_driver.f90 $(DEPENDLIB) mod_atmos_vars.o
$(BUILD_DIR)/mod_atmos_vars_sf.o          : mod_atmos_vars_sf.f90 $(DEPENDLIB) 
$(BUILD_DIR)/mod_atmos_phy_sf_driver.o    : mod_atmos_phy_sf_driver.f90 $(DEPENDLIB) mod_atmos_vars.o mod_atmos_vars_sf.o mod_cpl_vars.o
$(BUILD_DIR)/mod_atmos_phy_mp_driver.o    : mod_atmos_phy_mp_driver.f90 $(DEPENDLIB) mod_atmos_vars.o
$(BUILD_DIR)/mod_atmos_phy_tb_driver.o    : mod_atmos_phy_tb_driver.f90 $(DEPENDLIB) mod_atmos_vars.o
$(BUILD_DIR)/mod_atmos_phy_rd_driver.o    : mod_atmos_phy_rd_driver.f90 $(DEPENDLIB) mod_atmos_vars.o
$(BUILD_DIR)/mod_atmos_phy_ae_driver.o    : mod_atmos_phy_ae_driver.f90 $(DEPENDLIB) mod_atmos_vars.o
$(BUILD_DIR)/mod_atmos_vars.o             : mod_atmos_vars.f90 $(DEPENDLIB) 
$(BUILD_DIR)/mod_atmos.o                  : mod_atmos.f90 $(DEPENDLIB) mod_atmos_vars.o mod_cpl_vars.o \
                                            mod_atmos_dyn_driver.o mod_atmos_phy_sf_driver.o mod_atmos_phy_mp_driver.o mod_atmos_phy_tb_driver.o mod_atmos_phy_rd_driver.o mod_atmos_phy_ae_driver.o

$(BUILD_DIR)/mod_land_phy_bucket.o        : mod_land_phy_bucket.f90 $(DEPENDLIB) mod_land_vars.o
$(BUILD_DIR)/mod_land_vars.o              : mod_land_vars.f90 $(DEPENDLIB) 
$(BUILD_DIR)/mod_land.o                   : mod_land.f90 $(DEPENDLIB) mod_land_vars.o mod_cpl_vars.o mod_land_phy_bucket.o

$(BUILD_DIR)/mod_ocean_phy_slab.o         : mod_ocean_phy_slab.f90 $(DEPENDLIB) mod_ocean_vars.o
$(BUILD_DIR)/mod_ocean_vars.o             : mod_ocean_vars.f90 $(DEPENDLIB) 
$(BUILD_DIR)/mod_ocean.o                  : mod_ocean.f90 $(DEPENDLIB) mod_ocean_vars.o mod_cpl_vars.o mod_ocean_phy_slab.o

$(BUILD_DIR)/mod_cpl_atmos_land_driver.o  : mod_cpl_atmos_land_driver.f90 $(DEPENDLIB) mod_cpl_vars.o mod_land_phy_bucket.o mod_atmos_phy_sf_driver.o
$(BUILD_DIR)/mod_cpl_atmos_ocean_driver.o : mod_cpl_atmos_ocean_driver.f90 $(DEPENDLIB) mod_cpl_vars.o mod_ocean_phy_slab.o mod_atmos_phy_sf_driver.o
$(BUILD_DIR)/mod_cpl_vars.o               : mod_cpl_vars.f90 $(DEPENDLIB) 
$(BUILD_DIR)/mod_cpl.o                    : mod_cpl.f90 $(DEPENDLIB)  mod_cpl_vars.o mod_cpl_atmos_land_driver.o mod_cpl_atmos_ocean_driver.o

$(BUILD_DIR)/mod_mktopo.o                 : mod_mktopo.f90 $(DEPENDLIB) 
$(BUILD_DIR)/mod_mkinit.o                 : mod_mkinit.f90 $(DEPENDLIB) mod_atmos_vars.o
$(BUILD_DIR)/mod_cnvboundary.o            : mod_cnvboundary.f90 $(DEPENDLIB) 

$(BUILD_DIR)/mod_user.o                   : mod_user.f90 $(DEPENDLIB) mod_atmos_vars.o



allclean: cleanscalelib distclean

cleanscalelib:
	$(MAKE) -C $(SCALELIBDIR)/src allclean

distclean: clean
	rm -f $(PRJS)

clean:
	rm -f $(PRJS) $(BUILD_DIR)/*.h $(BUILD_DIR)/*.mod $(BUILD_DIR)/*.o $(BUILD_DIR)/*.lst



.SUFFIXES:
.SUFFIXES: .o .f90 .c

ifeq ($(SDM),-D_SDM)
$(BUILD_DIR)/%.o : %.f90
	$(FC) $(FFLAGS) $(ADDITIONAL_FFLAGS) -DVERSION_MACRO=\"$(VERSION)\" -I$(BUILD_DIR) -I$(DCUTILSDIR) -I$(GTOOLDIR) $(CONTRIB_INCLUDE) -I$(SCALELIBDIR)/include -I$(MODDIR) $(PAPI_INCLUDE) -o $@ -c $< $(MODDIROPT) $(BUILD_DIR)
else
$(BUILD_DIR)/%.o : %.f90
	$(FC) $(FFLAGS) $(ADDITIONAL_FFLAGS) -DVERSION_MACRO=\"$(VERSION)\" -I$(BUILD_DIR) -I$(DCUTILSDIR) -I$(GTOOLDIR) -I$(SCALELIBDIR)/include -I$(MODDIR) $(PAPI_INCLUDE) -o $@ -c $< $(MODDIROPT) $(BUILD_DIR)
endif
$(BUILD_DIR)/%.o : %.c
	$(CC) $(CFLAGS) -o $@ -c $< $(MODDIROPT) $(BUILD_DIR)

.PHONY: clean
