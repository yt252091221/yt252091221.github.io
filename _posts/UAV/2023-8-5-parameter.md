# 参数设置

|                  基本参数                   |    值    |
| :-----------------------------------------: | :------: |
|               CBRK_IO_SAFETY                |  22027   |
|     SYS_USE_IO（确保电机可以控制转速）      |    0     |
|                DSHOT_CONFIG                 | Dshot600 |
|                CBRK_USB_CHK                 |  197848  |
|               CBRK_SUPPLY_CHK               |  894281  |
|                SER_TEL1_BUAD                |  921600  |
|                   TELEM2                    |   打开   |
|                                             |          |
|         *（务必确认位置信息来源）*          |          |
|                     AID                     |   GPS    |
|                     HGT                     |   GPS    |
|                                             |          |
|                 **GPS参数**                 |  **值**  |
|       Survey in accuracy（最小精度）        | 2 / 2.5  |
| Minimum observation duration(最小持续时间） |    40    |
|                MAV_PROTO_VER                |    2     |
|              EKF2_GPS_V_NOISE               |   0.2    |
|              EKF2_GPS_P_NOISE               |   0.2    |
|                                             |          |
|                 **PID参数**                 |  **值**  |
|                 MC_PITCH_P                  |    7     |
|               MC_PITCHRATE_P                |   0.15   |
|               MC_PITCHRATE_I                |   0.05   |
|               MC_PITCHRATE_D                |  0.003   |
|                                             |          |

# 电机方向

## 注： 1 / 2 电机需要垫片

![1](/parameter/1.png)
