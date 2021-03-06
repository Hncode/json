##########################################################################
# unit tests
##########################################################################

# additional flags
CXXFLAGS += -std=c++11 -Wall -Wextra -pedantic -Weffc++ -Wcast-align -Wcast-qual -Wno-ctor-dtor-privacy -Wdisabled-optimization -Wformat=2 -Winit-self -Wmissing-declarations -Wmissing-include-dirs -Wold-style-cast -Woverloaded-virtual -Wredundant-decls -Wshadow -Wsign-conversion -Wsign-promo -Wstrict-overflow=5 -Wswitch -Wundef -Wno-unused -Wnon-virtual-dtor -Wreorder -Wdeprecated -Wno-float-equal
CPPFLAGS += -I ../src -I . -I thirdparty/catch

SOURCES = src/unit.cpp \
          src/unit-algorithms.cpp \
          src/unit-allocator.cpp \
          src/unit-capacity.cpp \
          src/unit-cbor.cpp \
          src/unit-class_const_iterator.cpp \
          src/unit-class_iterator.cpp \
          src/unit-class_lexer.cpp \
          src/unit-class_parser.cpp \
          src/unit-comparison.cpp \
          src/unit-concepts.cpp \
          src/unit-constructor1.cpp \
          src/unit-constructor2.cpp \
          src/unit-convenience.cpp \
          src/unit-conversions.cpp \
          src/unit-deserialization.cpp \
          src/unit-element_access1.cpp \
          src/unit-element_access2.cpp \
          src/unit-inspection.cpp \
          src/unit-iterator_wrapper.cpp \
          src/unit-iterators1.cpp \
          src/unit-iterators2.cpp \
          src/unit-json_patch.cpp \
          src/unit-json_pointer.cpp \
          src/unit-modifiers.cpp \
          src/unit-msgpack.cpp \
          src/unit-pointer_access.cpp \
          src/unit-readme.cpp \
          src/unit-reference_access.cpp \
          src/unit-regression.cpp \
          src/unit-serialization.cpp \
          src/unit-unicode.cpp \
          src/unit-testsuites.cpp

OBJECTS = $(SOURCES:.cpp=.o)

TESTCASES = $(patsubst src/unit-%.cpp,test-%,$(wildcard src/unit-*.cpp))

##############################################################################
# main rules
##############################################################################

all: $(TESTCASES)

clean:
	rm -fr json_unit $(OBJECTS) $(SOURCES:.cpp=.gcno) $(SOURCES:.cpp=.gcda) $(TESTCASES)

##############################################################################
# single test file
##############################################################################

json_unit: $(OBJECTS) ../src/json.hpp thirdparty/catch/catch.hpp
	@echo "[CXXLD] $@"
	@$(CXX) $(CXXFLAGS) $(LDFLAGS) $(OBJECTS) -o $@

%.o: %.cpp ../src/json.hpp thirdparty/catch/catch.hpp
	@echo "[CXX]   $@"
	@$(CXX) $(CXXFLAGS) $(CPPFLAGS) -c $< -o $@


##############################################################################
# individual test cases
##############################################################################

test-%: src/unit-%.cpp ../src/json.hpp thirdparty/catch/catch.hpp
	@echo "[CXXLD] $@"
	@$(CXX) $(CXXFLAGS) $(CPPFLAGS) $(LDFLAGS) -DCATCH_CONFIG_MAIN $< -o $@

TEST_PATTERN = "*"
TEST_PREFIX = ""
check: $(TESTCASES)
	@cd .. ; for testcase in $(TESTCASES); do echo "Executing $$testcase..."; $(TEST_PREFIX)test/$$testcase $(TEST_PATTERN) || exit 1; done


##############################################################################
# fuzzer
##############################################################################

parse_afl_fuzzer:
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) src/fuzzer-driver_afl.cpp src/fuzzer-parse_json.cpp -o $@

parse_cbor_fuzzer:
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) src/fuzzer-driver_afl.cpp src/fuzzer-parse_cbor.cpp -o $@

parse_msgpack_fuzzer:
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) src/fuzzer-driver_afl.cpp src/fuzzer-parse_msgpack.cpp -o $@
