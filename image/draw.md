Package draw

Overview ▾

Package draw provides image composition functions.
See "The Go image/draw package" for an introduction to this package: http://golang.org/doc/articles/image_draw.html
draw包提供了图像合成功能。

func Draw

func Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point, op Op)
Draw calls DrawMask with a nil mask.
用 一个nilmask调用DrawMask

func DrawMask

func DrawMask(dst Image, r image.Rectangle, src image.Image, sp image.Point, mask image.Image, mp image.Point, op Op)
DrawMask aligns r.Min in dst with sp in src and mp in mask and then replaces the rectangle r in dst with the result of a Porter-Duff composition. 
A nil mask is treated as opaque.
对齐r.Min在DST与SP的src和MP ，然后替换dst中的矩形r与Porter-Duff组合的结果。

type Drawer

type Drawer interface {
        // Draw aligns r.Min in dst with sp in src and then replaces the
        // rectangle r in dst with the result of drawing src on dst.
          //对齐r.Min在DST与SP在SRC，然后替换dst中的矩形r与DST绘制SRC的结果。
        Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point)
}
Drawer contains the Draw method.
Drawer包含Draw方法

var FloydSteinberg Drawer = floydSteinberg{}
FloydSteinberg is a Drawer that is the Src Op with Floyd-Steinberg error diffusion.
FloydSteinberg是一个 Drawer 那是 Src Op 的 Floyd-Steinberg 误差扩散。


type Image

type Image interface {
        image.Image
        Set(x, y int, c color.Color)
}
Image is an image.Image with a Set method to change a single pixel.
一个image.Image的 Set 方法 用来 改变 单个像素

type Op

type Op int
Op is a Porter-Duff compositing operator.
是一个 Porter-Duff 合成运算符

const (
        // Over specifies ``(src in mask) over dst''.
        Over Op = iota
        // Src specifies ``src in mask''.
        Src
)
func (Op) Draw

func (op Op) Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point)
Draw implements the Drawer interface by calling the Draw function with this Op.
根据调用Draw 方法和这个 Op 来实现Drawer接口

type Quantizer

type Quantizer interface {
        // Quantize appends up to cap(p) - len(p) colors to p and returns the
        // updated palette suitable for converting m to a paletted image.
           //追加cap(p) - len(p)颜色到p 并且返回更新的调色板适合将m为调色板图像。
        Quantize(p color.Palette, m image.Image) color.Palette
}
Quantizer produces a palette for an image.
为图像产生一个调色板

















