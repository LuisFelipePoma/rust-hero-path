# 11 - Image Processor CLI

## Description
CLI tool for image processing: resize, crop, grayscale, filters, and format conversion.

## Usage Example
```bash
$ image-tool input.png --resize 800x600 --output output.jpg
$ image-tool input.png --grayscale --output gray.png
$ image-tool input.png --crop 100,100 400x400 --output cropped.png
$ image-tool input.png --blur 5 --sharpen --output sharp.png
$ image-tool --batch "*.jpg" --resize 1920x1080 --format png
```

## Concepts to Learn
- Image processing with the `image` crate
- Different image formats (PNG, JPEG, WebP, BMP, GIF)
- Pixel manipulation
- Batch processing
- CLI with multiple operations

## Processing Example
```rust
use image::{imageops, DynamicImage, GenericImageView};

fn process_image(
    img: DynamicImage,
    ops: &[ImageOperation],
) -> Result<DynamicImage, ImageError> {
    let mut current = img;
    
    for op in ops {
        current = match op {
            ImageOperation::Resize(w, h) => {
                current.resize(*w, *h, imageops::FilterType::Lanczos3)
            }
            ImageOperation::Grayscale => {
                DynamicImage::ImageLuma8(current.to_luma8())
            }
            ImageOperation::Blur(radius) => {
                current.blur(*radius)
            }
            ImageOperation::Rotate(deg) => {
                current.rotate(*deg as f32)
            }
        };
    }
    
    Ok(current)
}
```

## Suggested Structure
```
image-processor/
|-- src/
|   |-- main.rs
|   |-- processor.rs     # Operations pipeline
|   |-- operations.rs    # Operation definitions
|   |-- batch.rs         # Batch processing
|   `-- formats.rs       # Format detection and conversion
|-- Cargo.toml
`-- tests/
```

## Suggested Dependencies
- `image` - Image processing
- `clap` - Argument parsing

## Resources
- [image crate documentation](https://docs.rs/image/)
- [Image filtering algorithms](https://en.wikipedia.org/wiki/Kernel_(image_processing))

## Difficulty Level
4/5 - Advanced, image processing math
