#Package image

##Overview ▾

Package image implements a basic 2-D image library.

The fundamental interface is called Image. An Image contains colors, which are described in the image/color package.

Values of the Image interface are created either by calling functions such as NewRGBA and NewPaletted, 
	or by calling Decode on an io.Reader containing image data in a format such as GIF, JPEG or PNG. 
Decoding any particular image format requires the prior registration of a decoder function. 
Registration is typically automatic as a side effect of initializing that format's package so that, to decode a PNG image, it suffices to have

image包实现了基本的2-d图片库.
最根本的接口调用Image.一个Image 包含在 image/color包里描述的颜色
Image接口的值  根据 调用的函数创建 例如 NewRGBA 和 NewPaletted,或者调用  io.Reader 里的Decode 包含 图片数据形式 例如GIF, JPEG 或 PNG.
解码任何特定的图像格式需要的解码器功能的预先注册。
注册通常是自动初始化为这种格式的软件包的副作用 所以 解码PNG 图片, 它足以有

```golang
import _ "image/png"
```

in a program's main package. The _ means to import a package purely for its initialization side effects.
See "The Go image package" for more details: http://golang.org/doc/articles/image_package.html
在程序的main包.  _ 意味着 引入一个包 只使用 它初始化效果


##Variables

var (
        // Black is an opaque black uniform image.
        //Black是不透明的黑色均匀的图像。
        Black = NewUniform(color.Black)
        
        // White is an opaque white uniform image.
        //White是不透明的白均匀的图像。
        White = NewUniform(color.White)
        
        // Transparent is a fully transparent uniform image.
        //Transparent是一个完全透明的均匀的图像。
        Transparent = NewUniform(color.Transparent)
        
        // Opaque is a fully opaque uniform image.
        //Opaque是一个完全不透明均匀的图像。
        Opaque = NewUniform(color.Opaque)
)
var ErrFormat = errors.New("image: unknown format")
ErrFormat indicates that decoding encountered an unknown format.
ErrFormat  表示 解码遇到的未知的格式

func RegisterFormat

func RegisterFormat(name, magic string, decode func(io.Reader) (Image, error), decodeConfig func(io.Reader) (Config, error))
RegisterFormat registers an image format for use by Decode. 
Name is the name of the format,like "jpeg" or "png". 
Magic is the magic prefix that identifies the format's encoding. 
The magic string can contain "?" wildcards that each match any one byte. 
Decode is the function that decodes the encoded image. 
DecodeConfig is the function that decodes just its configuration.
RegisterFormat 使用Decode 注册一个图片格式 .
Name是格式的名称,如 "jpeg" 或 "png". 
Magic是识别格式编码的前缀
magic字符串可以包含"?" 通配符 来匹配任何一个字节
Decode 是 解码 编码图片的函数
DecodeConfig是解码它配置的函数


type Alpha

type Alpha struct {
        // Pix holds the image's pixels, as alpha values. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1].
        //Pix保持图片像素的alpha值.(x, y)的像素 从Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1]开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
        //Rect是 图片的边界
        Rect Rectangle
}
Alpha is an in-memory image whose At method returns color.Alpha values.
Alpha是一个 内存图片,其在方法返回color.Alpha值


func NewAlpha

func NewAlpha(r Rectangle) *Alpha
NewAlpha returns a new Alpha with the given bounds.
根据给定的边界 返沪i一个新的Alpha

func (*Alpha) At

func (p *Alpha) At(x, y int) color.Color


func (*Alpha) Bounds

func (p *Alpha) Bounds() Rectangle


func (*Alpha) ColorModel

func (p *Alpha) ColorModel() color.Model


func (*Alpha) Opaque

func (p *Alpha) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图片实体并且 报告它是否完全不透明.

func (*Alpha) PixOffset

func (p *Alpha) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引

func (*Alpha) Set

func (p *Alpha) Set(x, y int, c color.Color)


func (*Alpha) SetAlpha

func (p *Alpha) SetAlpha(x, y int, c color.Alpha)


func (*Alpha) SubImage

func (p *Alpha) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素


type Alpha16

type Alpha16 struct {
        // Pix holds the image's pixels, as alpha values in big-endian format. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*2].
       //Pix保持图片像素在big-endian格式的alpha值.(x, y)的像素 从Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*2]开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
        //Rect是 图片的边界
        Rect Rectangle
}
Alpha16 is an in-memory image whose At method returns color.Alpha64 values.
Alpha16是一个 内存图片,其在方法返回color.Alpha64值


func NewAlpha16

func NewAlpha16(r Rectangle) *Alpha16
NewAlpha16 returns a new Alpha16 with the given bounds.
根据给定的边界返回一个新的Alpha16

func (*Alpha16) At

func (p *Alpha16) At(x, y int) color.Color


func (*Alpha16) Bounds

func (p *Alpha16) Bounds() Rectangle


func (*Alpha16) ColorModel

func (p *Alpha16) ColorModel() color.Model


func (*Alpha16) Opaque

func (p *Alpha16) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图片实体并且 报告它是否完全不透明.


func (*Alpha16) PixOffset

func (p *Alpha16) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引

func (*Alpha16) Set

func (p *Alpha16) Set(x, y int, c color.Color)


func (*Alpha16) SetAlpha16

func (p *Alpha16) SetAlpha16(x, y int, c color.Alpha16)


func (*Alpha16) SubImage

func (p *Alpha16) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素


type Config

type Config struct {
        ColorModel    color.Model
        Width, Height int
}
Config holds an image's color model and dimensions.
图像的颜色模型和尺寸。


func DecodeConfig

func DecodeConfig(r io.Reader) (Config, string, error)
DecodeConfig decodes the color model and dimensions of an image that has been encoded in a registered format. 
The string returned is the format name used during format registration. 
Format registration is typically done by an init function in the codec-specific package.
解码一个在注册格式中编码过的 图像的 颜色模型和尺寸。
返回的字符串格式是注册时使用的格式名
格式注册通常是由在编解码器专用包一个init函数来完成。



type Gray

type Gray struct {
        // Pix holds the image's pixels, as gray values. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1].
        //Pix保持图片像素的灰度值。.(x, y)的像素 从Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1]开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
           //图像边界
        Rect Rectangle
}
Gray is an in-memory image whose At method returns color.Gray values.
Gray是一个 内存图像,其在方法返回color.Gray值

func NewGray

func NewGray(r Rectangle) *Gray
NewGray returns a new Gray with the given bounds.
使用给的边界 返回一个新的Gray

func (*Gray) At

func (p *Gray) At(x, y int) color.Color


func (*Gray) Bounds

func (p *Gray) Bounds() Rectangle


func (*Gray) ColorModel

func (p *Gray) ColorModel() color.Model


func (*Gray) Opaque

func (p *Gray) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图像实体并且 报告它是否完全不透明.

func (*Gray) PixOffset

func (p *Gray) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引

func (*Gray) Set

func (p *Gray) Set(x, y int, c color.Color)
func (*Gray) SetGray

func (p *Gray) SetGray(x, y int, c color.Gray)
func (*Gray) SubImage

func (p *Gray) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素


type Gray16

type Gray16 struct {
        // Pix holds the image's pixels, as gray values in big-endian format. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*2].
         //Pix保持图片像素big-endian格式的灰度值。.(x, y)的像素 从Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*2]开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
        Rect Rectangle
}
Gray16 is an in-memory image whose At method returns color.Gray16 values.
Gray16是一个 内存图像,其在方法返回color.Gray16值

func NewGray16

func NewGray16(r Rectangle) *Gray16
NewGray16 returns a new Gray16 with the given bounds.
用给定的边界返回一个新的Gray16

func (*Gray16) At

func (p *Gray16) At(x, y int) color.Color


func (*Gray16) Bounds

func (p *Gray16) Bounds() Rectangle


func (*Gray16) ColorModel

func (p *Gray16) ColorModel() color.Model


func (*Gray16) Opaque

func (p *Gray16) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图像实体并且 报告它是否完全不透明.

func (*Gray16) PixOffset

func (p *Gray16) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引

func (*Gray16) Set

func (p *Gray16) Set(x, y int, c color.Color)
func (*Gray16) SetGray16

func (p *Gray16) SetGray16(x, y int, c color.Gray16)
func (*Gray16) SubImage

func (p *Gray16) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素



type Image

type Image interface {
        // ColorModel returns the Image's color model.
           //返回图像的颜色模型
        ColorModel() color.Model
        
        // Bounds returns the domain for which At can return non-zero color.
        // The bounds do not necessarily contain the point (0, 0).
          //返回 非零 颜色 的范围, 边界不必 包含(0, 0)
        Bounds() Rectangle
        
        // At returns the color of the pixel at (x, y).
        // At(Bounds().Min.X, Bounds().Min.Y) returns the upper-left pixel of the grid.
        // At(Bounds().Max.X-1, Bounds().Max.Y-1) returns the lower-right one.
           //返回 像素  (x, y)的颜色
           //在(Bounds().Min.X, Bounds().Min.Y)  返回左上角的网格像素。
           //在(Bounds().Max.X-1, Bounds().Max.Y-1) 返回右下角的网格像素。
        At(x, y int) color.Color
}
Image is a finite rectangular grid of color.Color values taken from a color model.
Image是从一个颜色模型取color.Color值的有限的矩形网格。


func Decode

func Decode(r io.Reader) (Image, string, error)
Decode decodes an image that has been encoded in a registered format. 
The string returned is the format name used during format registration. 
Format registration is typically done by an init function in the codec- specific package.
对已被编码在一个登记的格式的图像进行解码。
返回的字符串格式注册时使用的格式名。
格式注册通常是由在编解码器专用包一个init函数来完成.

type NRGBA

type NRGBA struct {
        // Pix holds the image's pixels, in R, G, B, A order. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*4].
          //保持图像 像素的  R, G, B, A顺序. 像素(x, y)在 Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*4] 开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
           //图像边界
        Rect Rectangle
}
NRGBA is an in-memory image whose At method returns color.NRGBA values.
NRGBA是一个 内存图像,其在方法返回color.NRGBA值


func NewNRGBA

func NewNRGBA(r Rectangle) *NRGBA
NewNRGBA returns a new NRGBA with the given bounds.
用给定的边界返回一个新的NRGBA

func (*NRGBA) At

func (p *NRGBA) At(x, y int) color.Color


func (*NRGBA) Bounds

func (p *NRGBA) Bounds() Rectangle


func (*NRGBA) ColorModel

func (p *NRGBA) ColorModel() color.Model


func (*NRGBA) Opaque

func (p *NRGBA) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图像实体并且 报告它是否完全不透明.


func (*NRGBA) PixOffset

func (p *NRGBA) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引

func (*NRGBA) Set

func (p *NRGBA) Set(x, y int, c color.Color)


func (*NRGBA) SetNRGBA

func (p *NRGBA) SetNRGBA(x, y int, c color.NRGBA)


func (*NRGBA) SubImage

func (p *NRGBA) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素


type NRGBA64

type NRGBA64 struct {
        // Pix holds the image's pixels, in R, G, B, A order and big-endian format. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*8].
        //保持图像 像素的  R, G, B, A顺序和big-endian格式. 像素(x, y)在 Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*8] 开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
           //图像边界
        Rect Rectangle
}
NRGBA64 is an in-memory image whose At method returns color.NRGBA64 values.
NRGBA64是一个 内存图像,其在方法返回color.NRGBA64值


func NewNRGBA64

func NewNRGBA64(r Rectangle) *NRGBA64
NewNRGBA64 returns a new NRGBA64 with the given bounds.
用给定的边界返回一个新的NRGBA64

func (*NRGBA64) At

func (p *NRGBA64) At(x, y int) color.Color


func (*NRGBA64) Bounds

func (p *NRGBA64) Bounds() Rectangle


func (*NRGBA64) ColorModel

func (p *NRGBA64) ColorModel() color.Model


func (*NRGBA64) Opaque

func (p *NRGBA64) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图像实体并且 报告它是否完全不透明.

func (*NRGBA64) PixOffset

func (p *NRGBA64) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引

func (*NRGBA64) Set

func (p *NRGBA64) Set(x, y int, c color.Color)


func (*NRGBA64) SetNRGBA64

func (p *NRGBA64) SetNRGBA64(x, y int, c color.NRGBA64)


func (*NRGBA64) SubImage

func (p *NRGBA64) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素



type Paletted

type Paletted struct {
        // Pix holds the image's pixels, as palette indices. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1].
           //保持图像的像素调色板指数,像素(x, y)在 Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*1] 开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
           //图像边界
        Rect Rectangle
        
        // Palette is the image's palette.
          //图像调色板
        Palette color.Palette
}
Paletted is an in-memory image of uint8 indices into a given palette.
NRGBA是一个 给定调色板的 内存图像uint8指数

func NewPaletted

func NewPaletted(r Rectangle, p color.Palette) *Paletted
NewPaletted returns a new Paletted with the given width, height and palette.
用给定的 宽 高 调色板 返回一个新的Paletted

func (*Paletted) At

func (p *Paletted) At(x, y int) color.Color


func (*Paletted) Bounds

func (p *Paletted) Bounds() Rectangle


func (*Paletted) ColorIndexAt

func (p *Paletted) ColorIndexAt(x, y int) uint8


func (*Paletted) ColorModel

func (p *Paletted) ColorModel() color.Model


func (*Paletted) Opaque

func (p *Paletted) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图像实体并且 报告它是否完全不透明.

func (*Paletted) PixOffset

func (p *Paletted) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引

func (*Paletted) Set

func (p *Paletted) Set(x, y int, c color.Color)


func (*Paletted) SetColorIndex

func (p *Paletted) SetColorIndex(x, y int, index uint8)


func (*Paletted) SubImage

func (p *Paletted) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素


type PalettedImage

type PalettedImage interface {
        // ColorIndexAt returns the palette index of the pixel at (x, y).
           //返回调色板的 (x, y) 索引
        ColorIndexAt(x, y int) uint8
        Image
}
PalettedImage is an image whose colors may come from a limited palette. 
If m is a PalettedImage and m.ColorModel() returns a PalettedColorModel p, then m.At(x, y) should be equivalent to p[m.ColorIndexAt(x, y)]. 
If m's color model is not a PalettedColorModel, then ColorIndexAt's behavior is undefined.
可能来自一个有限的调色板的一个图像的颜色
如果m是一个PalettedImage 并且  m.ColorModel() 返回一个 PalettedColorModel p,然后  m.At(x, y)应该等价于p[m.ColorIndexAt(x, y)]
如果m是颜色模型 而不是一个PalettedColorModel,那ColorIndexAt 是 undefined



type Point

type Point struct {
        X, Y int
}
A Point is an X, Y coordinate pair. The axes increase right and down.
Point是一个X,Y坐标对.轴向右和向下增加。
```golang
var ZP Point
```
ZP is the zero Point.
ZP是一个 零 Point


func Pt

func Pt(X, Y int) Point
Pt is shorthand for Point{X, Y}.
Pt是shorthand的简写


func (Point) Add

func (p Point) Add(q Point) Point
Add returns the vector p+q.
Add返回 向量 p+q


func (Point) Div

func (p Point) Div(k int) Point
Div returns the vector p/k.
Div返回 向量p/k 

func (Point) Eq

func (p Point) Eq(q Point) bool
Eq reports whether p and q are equal.
报告 p和q是否相等

func (Point) In

func (p Point) In(r Rectangle) bool
In reports whether p is in r.
报告 p 是否在r里

func (Point) Mod

func (p Point) Mod(r Rectangle) Point
Mod returns the point q in r such that p.X-q.X is a multiple of r's width and p.Y-q.Y is a multiple of r's height.
返回q 在r里 .如: p.X-q.X为r的宽度的整数倍 并且p.Y-q.Y  为r的高度的整数倍


func (Point) Mul

func (p Point) Mul(k int) Point
Mul returns the vector p*k.
返回 向量  p*k


func (Point) String

func (p Point) String() string
String returns a string representation of p like "(3,4)".
返回 字符串表示的 p  如  "(3,4)"


func (Point) Sub

func (p Point) Sub(q Point) Point
Sub returns the vector p-q.
返回向量p-q



type RGBA

type RGBA struct {
        // Pix holds the image's pixels, in R, G, B, A order. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*4].
           // 保持图像像素的 R, G, B, A  顺序.像素(x, y)在 Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*4] 开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
        //图像的边界
        Rect Rectangle
}
RGBA is an in-memory image whose At method returns color.RGBA values.
RGBA是一个 内存图像,其在方法返回color.RGBA值


func NewRGBA

func NewRGBA(r Rectangle) *RGBA
NewRGBA returns a new RGBA with the given bounds.
用给定的边界返回一个新的RGBA


func (*RGBA) At

func (p *RGBA) At(x, y int) color.Color


func (*RGBA) Bounds

func (p *RGBA) Bounds() Rectangle


func (*RGBA) ColorModel

func (p *RGBA) ColorModel() color.Model


func (*RGBA) Opaque

func (p *RGBA) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图像实体并且 报告它是否完全不透明.

func (*RGBA) PixOffset

func (p *RGBA) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引


func (*RGBA) Set

func (p *RGBA) Set(x, y int, c color.Color)
func (*RGBA) SetRGBA

func (p *RGBA) SetRGBA(x, y int, c color.RGBA)
func (*RGBA) SubImage

func (p *RGBA) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素



type RGBA64

type RGBA64 struct {
        // Pix holds the image's pixels, in R, G, B, A order and big-endian format. The pixel at
        // (x, y) starts at Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*8].
           // 保持图像像素的 R, G, B, A  顺序.像素(x, y)在 Pix[(y-Rect.Min.Y)*Stride + (x-Rect.Min.X)*8] 开始
        Pix []uint8
        
        // Stride is the Pix stride (in bytes) between vertically adjacent pixels.
        //Stride是垂直相邻像素的跨度
        Stride int
        
        // Rect is the image's bounds.
           //图像边界
        Rect Rectangle
}
RGBA64 is an in-memory image whose At method returns color.RGBA64 values.
RGBA64是一个 内存图像,其在方法返回color.RGBA64值


func NewRGBA64

func NewRGBA64(r Rectangle) *RGBA64
NewRGBA64 returns a new RGBA64 with the given bounds.
用给定的边界返回一个新的NRGBA

func (*RGBA64) At

func (p *RGBA64) At(x, y int) color.Color


func (*RGBA64) Bounds

func (p *RGBA64) Bounds() Rectangle


func (*RGBA64) ColorModel

func (p *RGBA64) ColorModel() color.Model


func (*RGBA64) Opaque

func (p *RGBA64) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
描图像实体并且 报告它是否完全不透明.

func (*RGBA64) PixOffset

func (p *RGBA64) PixOffset(x, y int) int
PixOffset returns the index of the first element of Pix that corresponds to the pixel at (x, y).
返回Pix 对应的 像素的 在(x, y) 的第一个元素的索引

func (*RGBA64) Set

func (p *RGBA64) Set(x, y int, c color.Color)


func (*RGBA64) SetRGBA64

func (p *RGBA64) SetRGBA64(x, y int, c color.RGBA64)


func (*RGBA64) SubImage

func (p *RGBA64) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r. 
The returned value shares pixels with the original image.
返回一个表示 图像P经由r可见的部分 的图片
返回值和原图 共享像素


type Rectangle

type Rectangle struct {
        Min, Max Point
}
A Rectangle contains the points with Min.X <= X < Max.X, Min.Y <= Y < Max.Y. 
It is well-formed if Min.X <= Max.X and likewise for Y. 
Points are always well-formed. A rectangle's methods always return well-formed outputs for well-formed inputs.
Rectangle 包含 Min.X <= X < Max.X, Min.Y <= Y < Max.Y. 
这是良好的 如果Min.X <= Max.X  并且 Y也是一样的
点始终是良好的,rectangle 方法 对 良好的输入 总会返回 良好的输出

var ZR Rectangle

ZR is the zero Rectangle.

func Rect

func Rect(x0, y0, x1, y1 int) Rectangle
Rect is shorthand for Rectangle{Pt(x0, y0), Pt(x1, y1)}.
Rectangle{Pt(x0, y0), Pt(x1, y1)}的简写


func (Rectangle) Add

func (r Rectangle) Add(p Point) Rectangle
Add returns the rectangle r translated by p.
返回矩形r转换为p。


func (Rectangle) Canon

func (r Rectangle) Canon() Rectangle
Canon returns the canonical version of r. 
The returned rectangle has minimum and maximum coordinates swapped if necessary so that it is well-formed.
返回r的规范版本。
返回值如果 需要的话 矩形有交换的最小和最大坐标 那样 它是好的


func (Rectangle) Dx

func (r Rectangle) Dx() int
Dx returns r's width.
返回r的宽度

func (Rectangle) Dy

func (r Rectangle) Dy() int
Dy returns r's height.
返回r的高

func (Rectangle) Empty

func (r Rectangle) Empty() bool
Empty reports whether the rectangle contains no points.
报告矩形是否包含任何点。

func (Rectangle) Eq

func (r Rectangle) Eq(s Rectangle) bool
Eq reports whether r and s are equal.
报告r和s是否相等

func (Rectangle) In

func (r Rectangle) In(s Rectangle) bool
In reports whether every point in r is in s.
报告r是否有任何点在s

func (Rectangle) Inset

func (r Rectangle) Inset(n int) Rectangle
Inset returns the rectangle r inset by n, which may be negative. 
If either of r's dimensions is less than 2*n then an empty rectangle near the center of r will be returned.
返回n个分段的r,可能是负值.
如果r尺寸小于2*n其后在接近R的中心一个空矩形将被退回。

func (Rectangle) Intersect

func (r Rectangle) Intersect(s Rectangle) Rectangle
Intersect returns the largest rectangle contained by both r and s. 
If the two rectangles do not overlap then the zero rectangle will be returned.
返回包含由两个r和s的最大矩形。
如果两个矩形没有重叠 将会返回零 矩形

func (Rectangle) Overlaps

func (r Rectangle) Overlaps(s Rectangle) bool
Overlaps reports whether r and s have a non-empty intersection.
报告r和s是否具有非空交集。

func (Rectangle) Size

func (r Rectangle) Size() Point
Size returns r's width and height.
返回r的宽和高

func (Rectangle) String

func (r Rectangle) String() string
String returns a string representation of r like "(3,4)-(6,5)".
返回 字符串表示的 r 如  "(3,4)-(6,5)"

func (Rectangle) Sub

func (r Rectangle) Sub(p Point) Rectangle
Sub returns the rectangle r translated by -p.
返回矩形r翻译-P。

func (Rectangle) Union

func (r Rectangle) Union(s Rectangle) Rectangle
Union returns the smallest rectangle that contains both r and s.
返回包含两个r和s的最小矩形。



type Uniform

type Uniform struct {
        C color.Color
}
Uniform is an infinite-sized Image of uniform color. 
It implements the color.Color, color.Model, and Image interfaces.
为色泽均匀的无限大小的图片。
它实现了 color.Color, color.Model, 和 Image 接口

func NewUniform

func NewUniform(c color.Color) *Uniform


func (*Uniform) At

func (c *Uniform) At(x, y int) color.Color


func (*Uniform) Bounds

func (c *Uniform) Bounds() Rectangle


func (*Uniform) ColorModel

func (c *Uniform) ColorModel() color.Model


func (*Uniform) Convert

func (c *Uniform) Convert(color.Color) color.Color


func (*Uniform) Opaque

func (c *Uniform) Opaque() bool
Opaque scans the entire image and reports whether it is fully opaque.
扫描整个图像和报告是否是完全不透明的。

func (*Uniform) RGBA

func (c *Uniform) RGBA() (r, g, b, a uint32)



type YCbCr

type YCbCr struct {
        Y, Cb, Cr      []uint8
        YStride        int
        CStride        int
        SubsampleRatio YCbCrSubsampleRatio
        Rect           Rectangle
}
YCbCr is an in-memory image of Y'CbCr colors. 
There is one Y sample per pixel, but each Cb and Cr sample can span one or more pixels. 
YStride is the Y slice index delta between vertically adjacent pixels. 
CStride is the Cb and Cr slice index delta between vertically adjacent pixels that map to separate chroma samples. 
It is not an absolute requirement, but YStride and len(Y) are typically multiples of 8, and:
是在内存中图像的Y'CbCr的颜色。
有每像素一个Y样本,但每个Cb和Cr采样可以跨越一个或多个像素。
YStride是垂直相邻的像素之间在Y分片索引。
CStride是映射到单独的色度样本垂直相邻的像素之间的Cb和Cr片折射率增量。
这不是一个绝对的要求，但YStride和len（Y）通常是8的倍数，并且：

For 4:4:4, CStride == YStride/1 && len(Cb) == len(Cr) == len(Y)/1.
For 4:2:2, CStride == YStride/2 && len(Cb) == len(Cr) == len(Y)/2.
For 4:2:0, CStride == YStride/2 && len(Cb) == len(Cr) == len(Y)/4.
For 4:4:0, CStride == YStride/1 && len(Cb) == len(Cr) == len(Y)/2.
func NewYCbCr

func NewYCbCr(r Rectangle, subsampleRatio YCbCrSubsampleRatio) *YCbCr
NewYCbCr returns a new YCbCr with the given bounds and subsample ratio.
返回一个新的YCbCr与给定的范围和子样本的比例。

func (*YCbCr) At

func (p *YCbCr) At(x, y int) color.Color
func (*YCbCr) Bounds

func (p *YCbCr) Bounds() Rectangle
func (*YCbCr) COffset

func (p *YCbCr) COffset(x, y int) int
COffset returns the index of the first element of Cb or Cr that corresponds to the pixel at (x, y).
返回CB或CR相对应的像素的（x，y）的第一个元素的索引。

func (*YCbCr) ColorModel

func (p *YCbCr) ColorModel() color.Model


func (*YCbCr) Opaque

func (p *YCbCr) Opaque() bool


func (*YCbCr) SubImage

func (p *YCbCr) SubImage(r Rectangle) Image
SubImage returns an image representing the portion of the image p visible through r.
The returned value shares pixels with the original image.
SubImage返回表示图像P经由r可见的部分的图像。
返回值与源图像 共享 像素

func (*YCbCr) YOffset

func (p *YCbCr) YOffset(x, y int) int
YOffset returns the index of the first element of Y that corresponds to the pixel at (x, y).
返回的Y对应于该像素的（x，y）的第一个元素的索引。

type YCbCrSubsampleRatio

type YCbCrSubsampleRatio int
YCbCrSubsampleRatio is the chroma subsample ratio used in a YCbCr image.
是在RGB图像中使用的色度二次采样比率。

const (
        YCbCrSubsampleRatio444 YCbCrSubsampleRatio = iota
        YCbCrSubsampleRatio422
        YCbCrSubsampleRatio420
        YCbCrSubsampleRatio440
)


func (YCbCrSubsampleRatio) String

func (s YCbCrSubsampleRatio) String() string

















































































































