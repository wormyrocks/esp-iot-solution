set(srcs "src/hal_driver.c"
        "src/lightbulb.c"
        "src/lighting.c"
        )

set(priv_req nvs_flash driver esp_timer)

if("${IDF_VERSION_MAJOR}.${IDF_VERSION_MINOR}" VERSION_GREATER_EQUAL "5.2.6")
list(APPEND priv_req "esp_driver_i2c")
endif()

set(incs "include" "drivers/common/utils/")

if(CONFIG_ENABLE_PWM_DRIVER)
    list(APPEND srcs "drivers/pwm/pwm.c")
    list(APPEND incs "drivers/pwm")
endif()

if(CONFIG_ENABLE_SM2135EH_DRIVER)
    list(APPEND srcs "drivers/sm2135eh/sm2135eh.c")
    list(APPEND incs "drivers/sm2135eh")
    list(APPEND srcs "drivers/common/iic/iic.c")
    list(APPEND incs "drivers/common/iic/")
endif()

if(CONFIG_ENABLE_SM2182E_DRIVER)
    list(APPEND srcs "drivers/sm2182e/sm2182e.c")
    list(APPEND incs "drivers/sm2182e")
    list(APPEND srcs "drivers/common/iic/iic.c")
    list(APPEND incs "drivers/common/iic/")
endif()

if(CONFIG_ENABLE_SM2x35EGH_DRIVER)
    list(APPEND srcs "drivers/sm2x35egh/sm2x35egh.c")
    list(APPEND incs "drivers/sm2x35egh")
    list(APPEND srcs "drivers/common/iic/iic.c")
    list(APPEND incs "drivers/common/iic/")
endif()

if(CONFIG_ENABLE_BP57x8D_DRIVER)
    list(APPEND srcs "drivers/bp57x8d/bp57x8d.c")
    list(APPEND incs "drivers/bp57x8d")
    list(APPEND srcs "drivers/common/iic/iic.c")
    list(APPEND incs "drivers/common/iic/")
endif()

if(CONFIG_ENABLE_BP1658CJ_DRIVER)
    list(APPEND srcs "drivers/bp1658cj/bp1658cj.c")
    list(APPEND incs "drivers/bp1658cj")
    list(APPEND srcs "drivers/common/iic/iic.c")
    list(APPEND incs "drivers/common/iic/")
endif()

if(CONFIG_ENABLE_KP18058_DRIVER)
    list(APPEND srcs "drivers/kp18058/kp18058.c")
    list(APPEND incs "drivers/kp18058")
    list(APPEND srcs "drivers/common/iic/iic.c")
    list(APPEND incs "drivers/common/iic/")
endif()

if(CONFIG_ENABLE_WS2812_DRIVER)
    list(APPEND srcs "drivers/ws2812/ws2812.c")
    list(APPEND incs "drivers/ws2812")
endif()

idf_component_register(SRCS ${srcs}
                        INCLUDE_DIRS ${incs}
                        PRIV_INCLUDE_DIRS "src/priv_include"
                        PRIV_REQUIRES ${priv_req}
                        )

include(package_manager)
cu_pkg_define_version(${CMAKE_CURRENT_LIST_DIR})
