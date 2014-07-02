Package gif

Overview ▾

Package gif implements a GIF image decoder and encoder.

The GIF specification is at http://www.w3.org/Graphics/GIF/spec-gif89a.txt.
实现一个GIF图像 解码和编码


func Decode

func Decode(r io.Reader) (image.Image, error)
Decode reads a GIF image from r and returns the first embedded image as an image.Image.
从r中读取一个GIF 图像  并且返回第一个嵌入式图像作为image.Image.

func DecodeConfig

func DecodeConfig(r io.Reader) (image.Config, error)
DecodeConfig returns the global color model and dimensions of a GIF image without decoding the entire image.
返回全局的颜色模型,一个没有整个图像进行解码的GIF图像的尺寸

func Encode

func Encode(w io.Writer, m image.Image, o *Options) error
Encode writes the Image m to w in GIF format.
以 GIF格式 把Image m  写入到w

func EncodeAll

func EncodeAll(w io.Writer, g *GIF) error
EncodeAll writes the images in g to w in GIF format with the given loop count and delay between frames.
用给定的循环次数与帧之间的延迟 以GIF 格式  把图像 g 写到 w

type GIF

type GIF struct {
        Image     []*image.Paletted // The successive images.
        Delay     []int             // The successive delay times, one per frame, in 100ths of a second.
        LoopCount int               // The loop count.
}
GIF represents the possibly multiple images stored in a GIF file.
表示 存储在GIF文件里的为多个图像

func DecodeAll

func DecodeAll(r io.Reader) (*GIF, error)
DecodeAll reads a GIF image from r and returns the sequential frames and timing information.
从r中读取GIF文件 并且返回在连续的帧和定时信息。

type Options

type Options struct {
        // NumColors is the maximum number of colors used in the image.
        // It ranges from 1 to 256.
           // NumColors图像里使用的最大颜色数,范围从1 到256
        NumColors int

        // Quantizer is used to produce a palette with size NumColors.
        // palette.Plan9 is used in place of a nil Quantizer.
        // Quantizer用来产生NumColors大小的 调色板
        //palette.Plan9 用来 代替一个 nilQuantizer
        Quantizer draw.Quantizer

        // Drawer is used to convert the source image to the desired palette.
        // draw.FloydSteinberg is used in place of a nil Drawer.
       // Drawer用于将源图像转换成所希望的调色板。
       //draw.FloydSteinberg用来代替一个nil Drawer
        Drawer draw.Drawer
}
Options are the encoding parameters.
Options是编码参数

