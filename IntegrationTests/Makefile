CONFIGURATION ?= debug

FORCE:
TestSuites/.build/$(CONFIGURATION)/%: FORCE
	swift build --package-path TestSuites \
	            --product $(notdir $@) \
	            --triple wasm32-unknown-wasi \
	            --configuration $(CONFIGURATION)

dist/%.wasm: TestSuites/.build/$(CONFIGURATION)/%
	mkdir -p dist
	cp $< $@

node_modules: package-lock.json
	npm install

.PHONY: build_rt
build_rt: node_modules
	cd .. && npm run build

.PHONY: benchmark_setup
benchmark_setup: build_rt dist/BenchmarkTests.wasm

.PHONY: run_benchmark
run_benchmark:
	node bin/benchmark-tests.js

.PHONY: benchmark
benchmark: benchmark_setup run_benchmark

.PHONY: test
test: build_rt dist/PrimaryTests.wasm
	node bin/primary-tests.js
