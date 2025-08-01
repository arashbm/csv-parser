include(FetchContent)

FetchContent_Declare(
  Catch2
  GIT_REPOSITORY https://github.com/catchorg/Catch2.git
  GIT_TAG v3.6.0
) 

FetchContent_MakeAvailable(Catch2)

add_executable(csv_test "")
target_sources(csv_test
    PRIVATE
        ${CSV_INCLUDE_DIR}/csv.hpp
        main.cpp
        test_csv_field.cpp
        test_csv_field_array.cpp
        test_csv_format.cpp
        test_csv_iterator.cpp
        test_csv_row.cpp
        test_csv_row_json.cpp
        test_csv_stat.cpp
        test_guess_csv.cpp
        test_read_csv.cpp
        test_read_csv_file.cpp
        test_write_csv.cpp
        test_data_type.cpp
        test_raw_csv_data.cpp
        test_round_trip.cpp
        test_csv_delimeter.cpp
        test_csv_ranges.cpp
    )
target_link_libraries(csv_test csv)
target_link_libraries(csv_test Catch2::Catch2WithMain)

if(MSVC)
    # Workaround to enable debugging unit tests in Visual Studio
    add_custom_command(
        TARGET csv_test POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CSV_TEST_DIR}/data $<TARGET_FILE_DIR:csv_test>/tests/data
    )
endif()

add_test(
    NAME test
    COMMAND csv_test
    WORKING_DIRECTORY ${CSV_ROOT_DIR}
)