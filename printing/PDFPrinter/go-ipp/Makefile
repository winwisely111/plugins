#  https://github.com/phin1x/go-ipp

LIB_NAME=go-ipp
LIB=github.com/phin1x/$(LIB_NAME)
LIB_BRANCH=master
LIB_FSPATH=$(GOPATH)/src/$(LIB)

GO111MODULE=on



help:  ## Display this help
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make \033[36m<target>\033[0m\n"} /^[a-zA-Z_-]+:.*?##/ { printf "  \033[36m%-15s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)


print: ## print
	@echo
	@echo LIB_NAME: $(LIB_NAME)
	@echo LIB: $(LIB)
	@echo LIB_BRANCH: $(LIB_BRANCH)
	@echo LIB_FSPATH: $(LIB_FSPATH)
	@echo

git-clone:
	mkdir -p $(LIB_FSPATH)
	cd $(LIB_FSPATH) && cd .. && rm -rf $(LIB_NAME) && git clone ssh://git@$(LIB).git
	cd $(LIB_FSPATH) && git checkout $(LIB_BRANCH)
git-pull:
	cd $(LIB_FSPATH) && git pull
git-clean:
	rm -rf $(LIB_FSPATH)

code:
	code $(LIB_FSPATH)

os-dep:
	# Assume hover make file has ben run 


### Flu

flu-config: ## flu-config
	flutter config
	flutter doctor -v
	flutter devices

flu-config-desk: ## flu-config-desk
	hover -h




SAMPLE_NAME=enrollment
SAMPLE_FSPATH=$(LIB_FSPATH)/$(SAMPLE_NAME)

flu-print: ## flu-print
	@echo
	@echo SAMPLE_NAME: $(SAMPLE_NAME)
	@echo SAMPLE_FSPATH: $(SAMPLE_FSPATH)
	@echo
flu-clean:
	cd $(SAMPLE_FSPATH) && flutter clean
flu-update:
	cd $(SAMPLE_FSPATH) && flutter packages get



### DESK

flu-desk-local-run: ## flu-desk-local-run
	# does not use docker
	cd $(SAMPLE_FSPATH) && hover run

flu-desk-local-build: ## flu-desk-local-build
	# does not use docker
	cd $(SAMPLE_FSPATH) && hover build


flu-desk-build-all: ## flu-desk-build-all
	$(MAKE) flu-desk-build-clean
	$(MAKE) flu-desk-build-init
	$(MAKE) flu-desk-build

flu-desk-build-init: 
	cd $(SAMPLE_FSPATH) && hover init $(LIB)/$(SAMPLE_NAME)

flu-desk-build-clean:
	rm -rf $(SAMPLE_FSPATH)/go

	# this wipes ALL of docker data
	docker system prune
	#docker system prune --force


flu-desk-build:
	# uses docker to build

	cd $(SAMPLE_FSPATH) && hover build darwin
	cd $(SAMPLE_FSPATH) && hover build windows
	# broken: need to look into
	#cd $(SAMPLE_FSPATH) && hover build linux

flu-desk-buildrun: flu-desk-build

	# darwin
	open $(SAMPLE_FSPATH)/go/build/outputs/darwin/
	open $(SAMPLE_FSPATH)/go/build/outputs/darwin/$(SAMPLE)

### Desk Packaging

flu-desk-pack-all: ## flu-desk-pack-all
	$(MAKE) flu-desk-pack-clean
	$(MAKE) flu-desk-pack-init
	$(MAKE) flu-desk-pack

flu-desk-pack-clean:
	rm -rf $(SAMPLE_FSPATH)/go/packaging

	# this wipes ALL of docker data
	docker system prune
	#docker system prune --force

flu-desk-pack-init:

	# Creates any files needed to do packaging.
	# Only needed once.

	# hover init-packaging --help

	#darwin
	cd $(SAMPLE_FSPATH) && hover init-packaging darwin-bundle
	cd $(SAMPLE_FSPATH) && hover init-packaging darwin-pkg

	# windows
	cd $(SAMPLE_FSPATH) && hover init-packaging windows-msi

	# linux
	cd $(SAMPLE_FSPATH) && hover init-packaging linux-appimage
	cd $(SAMPLE_FSPATH) && hover init-packaging linux-deb
	cd $(SAMPLE_FSPATH) && hover init-packaging linux-snap

flu-desk-pack:

	# hover build --help

	# darwin
	cd $(SAMPLE_FSPATH) && hover build darwin-pkg
	
	# windows
	#cd $(SAMPLE_FSPATH) && hover build windows-msi

	# linux
	# broken
	#cd $(SAMPLE_FSPATH) && hover build linux-snap
	




### Desk Zipping

flu-desk-dist-all: ## flu-desk-dist-all
	$(MAKE) flu-desk-dist-clean
	$(MAKE) flu-desk-dist-zip
	$(MAKE) flu-desk-dist-unzip

flu-desk-dist-clean:
	rm -rf $(SAMPLE_FSPATH)/dist
	mkdir -p $(SAMPLE_FSPATH)/dist

flu-desk-dist-zip:
	# zip build by OS.

	# darwin
	cd $(SAMPLE_FSPATH)/go/build/outputs/darwin && zip -r -X $(SAMPLE_FSPATH)/dist/darwin.zip *

	# windows
	cd $(SAMPLE_FSPATH)/go/build/outputs/windows && zip -r -X $(SAMPLE_FSPATH)/dist/windows.zip *
	
	# linux
	cd $(SAMPLE_FSPATH)/go/build/outputs/linux && zip -r -X $(SAMPLE_FSPATH)/dist/linux.zip *
	

flu-desk-dist-unzip:
	# clean dist
	rm -rf $(SAMPLE_FSPATH)/dist/out
	mkdir -p $(SAMPLE_FSPATH)/dist/out


	# darwin
	unzip $(SAMPLE_FSPATH)/dist/darwin.zip -d $(SAMPLE_FSPATH)/dist/out/darwin

	# windows
	unzip $(SAMPLE_FSPATH)/dist/windows.zip -d $(SAMPLE_FSPATH)/dist/out/windows
	
	# linux
	unzip $(SAMPLE_FSPATH)/dist/linux.zip -d $(SAMPLE_FSPATH)/dist/out/linux

	

### Mob


flu-mob-run: ## flu-mob-run
	cd $(SAMPLE_FSPATH) && flutter run -d all

### i18n

#SAMPLE=keyboard_event
# works

#SAMPLE=mousebuttons
# works

#SAMPLE=pointer_demo
# works and is amazing
# Is perfect sampler, as it has: licenses, setting ( dark mode, debugging)
# TODO: The i18n code is there and working, but there is no easy way to change it in the GUI.
# TODO: Looks like i can use my Google trans gocode to gen more if i can parse the ARBS

stocks-i18n-step1:

	# generate ui code --> i18n code ( intl_messages.arb from lib/stock_strings.dart )
	cd $(LIB_FSPATH)/$(SAMPLE) && flutter pub pub run intl_translation:extract_to_arb --output-dir=lib/i18n lib/stock_strings.dart
	ls -al cd $(LIB_FSPATH)/$(SAMPLE)/lib/i18n

stocks-i18n-step2:
	# generate arb string --> i18n code (a stock_messages_<locale>.dart for each stocks_<locale>.arb file and stock_messages_all.dart)
	cd $(LIB_FSPATH)/$(SAMPLE) && flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/i18n \
   --generated-file-prefix=stock_ --no-use-deferred-loading lib/*.dart lib/i18n/stocks_*.arbs
    # Now you have all the "stock_messages_all, en, es.dart regenerated
	# Its used the stocks_en,es.arb as sources
	# Now each of the locale i18n dart code is populated with the translated string.
	
	


go-build:
	cd $(LIB_FSPATH) && go run main.go