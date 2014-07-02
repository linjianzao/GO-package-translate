#Package color

Overview ▾

Package color implements a basic color library.
实现 基础 颜色类库


Variables

var (
        Black       = Gray16{0}
        White       = Gray16{0xffff}
        Transparent = Alpha16{0}
        Opaque      = Alpha16{0xffff}
)
Standard colors.
标准颜色


func RGBToYCbCr

func RGBToYCbCr(r, g, b uint8) (uint8, uint8, uint8)
RGBToYCbCr converts an RGB triple to a Y'CbCr triple.
转换 一个RGB  到 Y'CbCr


func YCbCrToRGB

func YCbCrToRGB(y, cb, cr uint8) (uint8, uint8, uint8)
YCbCrToRGB converts a Y'CbCr triple to an RGB triple.
转换一个  Y'CbCr 到 RGB


type Alpha

type Alpha struct {
        A uint8
}
Alpha represents an 8-bit alpha color.
表示一个8位的alpha颜色

func (Alpha) RGBA

func (c Alpha) RGBA() (r, g, b, a uint32)


type Alpha16

type Alpha16 struct {
        A uint16
}
Alpha16 represents a 16-bit alpha color.
表示一个16位的alpha颜色

func (Alpha16) RGBA

func (c Alpha16) RGBA() (r, g, b, a uint32)



type Color

type Color interface {
        // RGBA returns the alpha-premultiplied red, green, blue and alpha values
        // for the color. Each value ranges within [0, 0xFFFF], but is represented
        // by a uint32 so that multiplying by a blend factor up to 0xFFFF will not
        // overflow.
        //RGBA返回 α-预乘 红 绿 蓝 和alpha 值 颜色. 每个值的范围 在[0, 0xFFFF]内,
           //但由一个UINT32表示，这样一个混合因子达至0xFFFF不会溢出相乘。
        RGBA() (r, g, b, a uint32)
}
Color can convert itself to alpha-premultiplied 16-bits per channel RGBA. 
The conversion may be lossy.
可以自己转换为α-预乘16位每通道的RGBA。
转换可能是有损耗的。


type Gray

type Gray struct {
        Y uint8
}
Gray represents an 8-bit grayscale color.
表示一个8位的 灰阶 颜色

func (Gray) RGBA

func (c Gray) RGBA() (r, g, b, a uint32)



type Gray16

type Gray16 struct {
        Y uint16
}
Gray16 represents a 16-bit grayscale color.
表示一个16位的 灰阶 颜色


func (Gray16) RGBA

func (c Gray16) RGBA() (r, g, b, a uint32)


type Model

type Model interface {
        Convert(c Color) Color
}
Model can convert any Color to one from its own color model. 
The conversion may be lossy.
可以转化任何的Color到 它 拥有的 颜色模型 中的一个. 转换可能是有损耗的。
var (
        RGBAModel    Model = ModelFunc(rgbaModel)
        RGBA64Model  Model = ModelFunc(rgba64Model)
        NRGBAModel   Model = ModelFunc(nrgbaModel)
        NRGBA64Model Model = ModelFunc(nrgba64Model)
        AlphaModel   Model = ModelFunc(alphaModel)
        Alpha16Model Model = ModelFunc(alpha16Model)
        GrayModel    Model = ModelFunc(grayModel)
        Gray16Model  Model = ModelFunc(gray16Model)
)
Models for the standard color types.
标准颜色类型

var YCbCrModel Model = ModelFunc(yCbCrModel)

YCbCrModel is the Model for Y'CbCr colors.
 Y'CbCr  颜色模型
 

func ModelFunc

func ModelFunc(f func(Color) Color) Model
ModelFunc returns a Model that invokes f to implement the conversion.
返回一个 调用 f实现转换的模型


type NRGBA

type NRGBA struct {
        R, G, B, A uint8
}
NRGBA represents a non-alpha-premultiplied 32-bit color.
表示非α-预乘32位色。

func (NRGBA) RGBA

func (c NRGBA) RGBA() (r, g, b, a uint32)


type NRGBA64

type NRGBA64 struct {
        R, G, B, A uint16
}
NRGBA64 represents a non-alpha-premultiplied 64-bit color, having 16 bits for each of red, green, blue and alpha.
代表一个非α-预乘64位颜色，具有16比特的每一个红，绿，蓝和alpha。

func (NRGBA64) RGBA

func (c NRGBA64) RGBA() (r, g, b, a uint32)



type Palette

type Palette []Color
Palette is a palette of colors.

func (Palette) Convert

func (p Palette) Convert(c Color) Color
Convert returns the palette color closest to c in Euclidean R,G,B space.
返回调色板的颜色最接近到c在欧几里德的R，G，B的空间。

func (Palette) Index

func (p Palette) Index(c Color) int
Index returns the index of the palette color closest to c in Euclidean R,G,B space.
返回调色板颜色的索引最接近到c中的欧几里德的R，G，B的空间。


type RGBA

type RGBA struct {
        R, G, B, A uint8
}
RGBA represents a traditional 32-bit alpha-premultiplied color, having 8 bits for each of red, green, blue and alpha.
代表了传统的32位alpha预乘颜色，有8位为每个红色，绿色，蓝色和alpha。

func (RGBA) RGBA

func (c RGBA) RGBA() (r, g, b, a uint32)



type RGBA64

type RGBA64 struct {
        R, G, B, A uint16
}
RGBA64 represents a 64-bit alpha-premultiplied color, having 16 bits for each of red, green, blue and alpha.
表示一个64位的α-预乘颜色，有16位为每个红色，绿色，蓝色和alpha。

func (RGBA64) RGBA

func (c RGBA64) RGBA() (r, g, b, a uint32)



type YCbCr

type YCbCr struct {
        Y, Cb, Cr uint8
}
YCbCr represents a fully opaque 24-bit Y'CbCr color, having 8 bits each for one luma and two chroma components.

JPEG, VP8, the MPEG family and other codecs use this color model. 
Such codecs often use the terms YUV and Y'CbCr interchangeably, but strictly speaking, 
	the term YUV applies only to analog video signals, and Y' (luma) is Y (luminance) after applying gamma correction.

Conversion between RGB and Y'CbCr is lossy and there are multiple, slightly different formulae for converting between the two. 
This package follows the JFIF specification at http://www.w3.org/Graphics/JPEG/jfif3.pdf.

表示完全不透明的24位Y'CbCr的颜色，是每一个8位的一个亮度和两个色度分量。
JPEG，VP8，将MPEG系列和其他编解码器使用此颜色模型。
这种编解码器经常使用的术语YUV和Y'CbCr的互换，但严格来说， 
术语YUV仅适用于模拟视频信号，并且Y'（亮度）是Y（亮度）将伽马校正后。
RGB和Y'CbCr的之间的转换是有损耗的，并有多重，略有不同的公式在两者之间转换。

func (YCbCr) RGBA

func (c YCbCr) RGBA() (uint32, uint32, uint32, uint32)












































