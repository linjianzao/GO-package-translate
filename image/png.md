Package png

Overview ▾

Package png implements a PNG image decoder and encoder.

The PNG specification is at http://www.w3.org/TR/PNG/.
png包 实现一个PNG 图像的 解码和编码


func Decode

func Decode(r io.Reader) (image.Image, error)
Decode reads a PNG image from r and returns it as an image.Image. 
The type of Image returned depends on the PNG contents.
从r中读取 PNG 图像 并且 返回它的 image.Image. 
返回图片的类型取决于PNG内容。


func DecodeConfig

func DecodeConfig(r io.Reader) (image.Config, error)
DecodeConfig returns the color model and dimensions of a PNG image without decoding the entire image.
返回颜色模型和PNG图像的尺寸没有整个图像进行解码。

func Encode

func Encode(w io.Writer, m image.Image) error
Encode writes the Image m to w in PNG format. 
Any Image may be encoded, but images that are not image.NRGBA might be encoded lossily.
以PNG格式 写入 图像m 到w
任何图像都可以编码, 但并非image.NRGBA图像可能损耗进行编码。

type FormatError

type FormatError string
A FormatError reports that the input is not a valid PNG.
报告 输入 不是一个有效的 PNG

func (FormatError) Error

func (e FormatError) Error() string


type UnsupportedError

type UnsupportedError string
An UnsupportedError reports that the input uses a valid but unimplemented PNG feature.
报告输入 是否是一个有效 但还没实现的 PNG功能

func (UnsupportedError) Error

func (e UnsupportedError) Error() string
