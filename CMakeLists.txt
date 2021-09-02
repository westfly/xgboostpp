CMAKE_MINIMUM_REQUIRED(VERSION 3.0.0)


PROJECT(xgboostpp)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -g")

add_executable(test_xgb test.cpp)
execute_process(
    COMMAND python3 -c "import sysconfig; print(sysconfig.get_paths()['purelib'], end='');"
    RESULT_VARIABLE RES
    OUTPUT_VARIABLE PYTHON_LIBRARY
    ERROR_QUIET
)
execute_process(
    COMMAND python3 -c "import xgboost as xgb; import os; print(os.path.dirname(xgb.__file__))"
    RESULT_VARIABLE RES
    OUTPUT_VARIABLE XGBOOST_DIR
    ERROR_QUIET
)
message("#### ${PYTHON_LIBRARY} ${XGBOOST_DIR}")
include_directories(${XGBOOST_DIR}/include ".")
link_directories(${XGBOOST_DIR}/lib)
find_library(XGBOOST xgboost HINTS "${XGBOOST_DIR}/lib")
target_include_directories(test_xgb PUBLIC ${XGBOOST_DIR}/include)
target_link_libraries(test_xgb PUBLIC ${XGBOOST})
