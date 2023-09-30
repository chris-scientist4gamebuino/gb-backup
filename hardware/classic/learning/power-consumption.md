
# Power consumption on Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Learning for Classic](./README.MD) | [Go to Classic Reference Summary](./../reference/README.MD)

**Default battery levels and thresholds values**

| **Level** | **Voltage**  | **Status**    | **Usual duration** |
|-----------|--------------|---------------|--------------------|
| 0         | <3500 mV     | Auto-shutdown |                    |
| 1         | 3500-3550 mV | Critical      | 30 minutes         |
| 2         | 3550-3700 mV | Empty         | 2 hours            |
| 3         | 3700-3900 mV | Half          | 8 hours            |
| 4         | >3900 mV     | Full          | 8 hours            |

Gamebuino consumes about 20mA with backlight, 18mA without and 11mA in power down mode.
