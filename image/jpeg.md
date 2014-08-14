Package jpeg

Overview ▾

Package jpeg implements a JPEG image decoder and encoder.

JPEG is defined in ITU-T T.81: http://www.w3.org/Graphics/JPEG/itu-t81.pdf.
实现一个 JPEG 图像 解码和编码

Constants

const DefaultQuality = 75
DefaultQuality is the default quality encoding parameter.
DefaultQuality是默认的编码质量参数。

func Decode

func Decode(r io.Reader) (image.Image, error)
Decode reads a JPEG image from r and returns it as an image.Image.
从r中读取一个 JPEG 图像 并且返回它 的image.Image.

func DecodeConfig

func DecodeConfig(r io.Reader) (image.Config, error)
DecodeConfig returns the color model and dimensions of a JPEG image without decoding the entire image.
返回颜色模型和一个没有整个图像进行解码的JPEG图像


func Encode

func Encode(w io.Writer, m image.Image, o *Options) error
Encode writes the Image m to w in JPEG 4:2:0 baseline format with the given options. 
Default parameters are used if a nil *Options is passed.
用给定的选项 以 JPEG 4:2:0 基线格式 把 图像m 写到 w

type FormatError

type FormatError string
A FormatError reports that the input is not a valid JPEG.
报告 输入是否是一个无效JPEG


func (FormatError) Error

func (e FormatError) Error() string
type Options

type Options struct {
        Quality int
}
Options are the encoding parameters. Quality ranges from 1 to 100 inclusive, higher is better.
Options编码参数.Quality 范围 从 1到100 , 越高表示越好

type Reader

type Reader interface {
        io.Reader
        ReadByte() (c byte, err error)
}
If the passed in io.Reader does not also have ReadByte, then Decode will introduce its own buffering.
如果传递到io.Reader 没有 ReadByte ,Decode 将会 采用  它本身的 缓冲区


type UnsupportedError

type UnsupportedError string
An UnsupportedError reports that the input uses a valid but unimplemented JPEG feature.
报告 输入是否是有效但是 是未实现的JPEG功能

func (UnsupportedError) Error

func (e UnsupportedError) Error() string