# ESP-BSP-RS

Rust Bare Metal Board Support Packages for ESP32 based boards with focus on Embassy Async

## List of boards

- [ESP32-C6-DevKit-C1](https://docs.espressif.com/projects/espressif-esp-dev-kits/en/latest/esp32c6/esp32-c6-devkitc-1/index.html)
- [M5Stack-CoreS3](https://shop.m5stack.com/products/m5stack-cores3-esp32s3-lotdevelopment-kit)

### Older boards

These boards are supported by the package, but it is not recommended to use them for new projects:

- [ESP32-S3-BOX](https://github.com/espressif/esp-box) - HW discontinued - replaced by ESP32-S3-BOX-3


## Usage

```
cargo add esp-bsp
```

### Display configuration


The configuration code for PINs intended to be used at main function
```rust
use esp_bsp_rs::lcd_gpios;

let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
let (lcd_sclk, lcd_mosi, lcd_cs, lcd_miso, lcd_dc, _lcd_backlight, lcd_reset) = lcd_gpios!(BoardType::ESP32C6DevKitC1, io);
```

The configuration code for Rust Embasy task
```rust
#[cfg(feature = "esp32_c6_devkit_c1")]
type AppDisplay = define_display_type!(BoardType::ESP32C6DevKitC1);
#[cfg(feature = "m5stack_cores3")]
type AppDisplay = define_display_type!(BoardType::M5StackCoreS3);
#[cfg(feature = "esp32_s3_box")]
type AppDisplay = define_display_type!(BoardType::ESP32S3Box);

#[embassy_executor::task]
pub async fn app_loop(mut display:AppDisplay)
```

If you intend to use suggested features then add similar configuration to Cargo.toml:
```
[dependencies]
esp32-hal = { version = "0.17.0", optional = true, default-features = false, features = ["embassy", "async", "embassy-time-timg0", "rt", "embassy-executor-thread"] }
esp32s2-hal = { version = "0.14.0", optional = true, default-features = false, features = ["embassy", "async", "embassy-time-timg0", "rt", "embassy-executor-thread"] }
esp32s3-hal = { version = "0.14.0", optional = true, default-features = false, features = ["embassy", "async", "embassy-time-timg0", "rt", "embassy-executor-thread"] }
esp32c3-hal = { version = "0.14.0", optional = true, default-features = false, features = ["embassy", "async", "embassy-time-timg0", "rt", "embassy-executor-thread"] }
esp32c6-hal = { version = "0.7.0", optional = true, default-features = false, features = ["embassy", "async", "embassy-time-timg0", "rt", "embassy-executor-thread"] }
esp32h2-hal = { version = "0.5.0", optional = true, default-features = false, features = ["embassy", "async", "embassy-time-timg0", "rt", "embassy-executor-thread"] }

[features]
esp32 =   [ "esp32-hal" ]
esp32s2 = [ "esp32s2-hal" ]
esp32s3 = [ "esp32s3-hal" ]
esp32c3 = [ "esp32c3-hal" ]
esp32c6 = [ "esp32c6-hal" ]
esp32h2 = [ "esp32h2-hal" ]

esp32_c6_devkit_c1 = [ "esp32c6" ]
esp32_s3_box = [ "esp32s3", "esp32s3-hal/opsram-8m" ]
m5stack_cores3 = [ "esp32s3", "esp32s3-hal/psram-8m" ]
```

## Change log

### 0.2.0

- renamed 
