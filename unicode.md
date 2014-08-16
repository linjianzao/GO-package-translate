Package unicode


Overview ▾

Package unicode provides data and functions to test some properties of Unicode code points.
unicode 包 提供数据和函数 来测试一些Unicode 字符属性


Constants
```golang
const (
        MaxRune         = '\U0010FFFF' // Maximum valid Unicode code point.
        ReplacementChar = '\uFFFD'     // Represents invalid code points.
        MaxASCII        = '\u007F'     // maximum ASCII value.
        MaxLatin1       = '\u00FF'     // maximum Latin-1 value.
)
```

```golang
const (
        UpperCase = iota
        LowerCase
        TitleCase
        MaxCase
)
```
Indices into the Delta arrays inside CaseRanges for case mapping.
指数映射 到Delta数组里 CaseRanges情况。

```golang
const (
        UpperLower = MaxRune + 1 // (Cannot be a valid delta.)
)
```
If the Delta field of a CaseRange is UpperLower, it means this CaseRange represents a sequence of the form (say) Upper Lower Upper Lower.
如果CaseRange 的 Delta 字段是 UpperLower, 它意味着 这个 CaseRange 表示  一个 Upper Lower Upper Lower 形式序列.

```golang
const Version = "6.3.0"
```
Version is the Unicode edition from which the tables are derived.
Version 是Unicode版本的表。


Variables
```golang
var (
        Cc     = _Cc // Cc is the set of Unicode characters in category Cc.
        Cf     = _Cf // Cf is the set of Unicode characters in category Cf.
        Co     = _Co // Co is the set of Unicode characters in category Co.
        Cs     = _Cs // Cs is the set of Unicode characters in category Cs.
        Digit  = _Nd // Digit is the set of Unicode characters with the "decimal digit" property.
        Nd     = _Nd // Nd is the set of Unicode characters in category Nd.
        Letter = _L  // Letter/L is the set of Unicode letters, category L.
        L      = _L
        Lm     = _Lm // Lm is the set of Unicode characters in category Lm.
        Lo     = _Lo // Lo is the set of Unicode characters in category Lo.
        Lower  = _Ll // Lower is the set of Unicode lower case letters.
        Ll     = _Ll // Ll is the set of Unicode characters in category Ll.
        Mark   = _M  // Mark/M is the set of Unicode mark characters, category M.
        M      = _M
        Mc     = _Mc // Mc is the set of Unicode characters in category Mc.
        Me     = _Me // Me is the set of Unicode characters in category Me.
        Mn     = _Mn // Mn is the set of Unicode characters in category Mn.
        Nl     = _Nl // Nl is the set of Unicode characters in category Nl.
        No     = _No // No is the set of Unicode characters in category No.
        Number = _N  // Number/N is the set of Unicode number characters, category N.
        N      = _N
        Other  = _C // Other/C is the set of Unicode control and special characters, category C.
        C      = _C
        Pc     = _Pc // Pc is the set of Unicode characters in category Pc.
        Pd     = _Pd // Pd is the set of Unicode characters in category Pd.
        Pe     = _Pe // Pe is the set of Unicode characters in category Pe.
        Pf     = _Pf // Pf is the set of Unicode characters in category Pf.
        Pi     = _Pi // Pi is the set of Unicode characters in category Pi.
        Po     = _Po // Po is the set of Unicode characters in category Po.
        Ps     = _Ps // Ps is the set of Unicode characters in category Ps.
        Punct  = _P  // Punct/P is the set of Unicode punctuation characters, category P.
        P      = _P
        Sc     = _Sc // Sc is the set of Unicode characters in category Sc.
        Sk     = _Sk // Sk is the set of Unicode characters in category Sk.
        Sm     = _Sm // Sm is the set of Unicode characters in category Sm.
        So     = _So // So is the set of Unicode characters in category So.
        Space  = _Z  // Space/Z is the set of Unicode space characters, category Z.
        Z      = _Z
        Symbol = _S // Symbol/S is the set of Unicode symbol characters, category S.
        S      = _S
        Title  = _Lt // Title is the set of Unicode title case letters.
        Lt     = _Lt // Lt is the set of Unicode characters in category Lt.
        Upper  = _Lu // Upper is the set of Unicode upper case letters.
        Lu     = _Lu // Lu is the set of Unicode characters in category Lu.
        Zl     = _Zl // Zl is the set of Unicode characters in category Zl.
        Zp     = _Zp // Zp is the set of Unicode characters in category Zp.
        Zs     = _Zs // Zs is the set of Unicode characters in category Zs.
)
```

These variables have type *RangeTable.
这些变量是  *RangeTable类型.


```golang
var (
        Arabic                 = _Arabic                 // Arabic is the set of Unicode characters in script Arabic.
        Armenian               = _Armenian               // Armenian is the set of Unicode characters in script Armenian.
        Avestan                = _Avestan                // Avestan is the set of Unicode characters in script Avestan.
        Balinese               = _Balinese               // Balinese is the set of Unicode characters in script Balinese.
        Bamum                  = _Bamum                  // Bamum is the set of Unicode characters in script Bamum.
        Batak                  = _Batak                  // Batak is the set of Unicode characters in script Batak.
        Bengali                = _Bengali                // Bengali is the set of Unicode characters in script Bengali.
        Bopomofo               = _Bopomofo               // Bopomofo is the set of Unicode characters in script Bopomofo.
        Brahmi                 = _Brahmi                 // Brahmi is the set of Unicode characters in script Brahmi.
        Braille                = _Braille                // Braille is the set of Unicode characters in script Braille.
        Buginese               = _Buginese               // Buginese is the set of Unicode characters in script Buginese.
        Buhid                  = _Buhid                  // Buhid is the set of Unicode characters in script Buhid.
        Canadian_Aboriginal    = _Canadian_Aboriginal    // Canadian_Aboriginal is the set of Unicode characters in script Canadian_Aboriginal.
        Carian                 = _Carian                 // Carian is the set of Unicode characters in script Carian.
        Chakma                 = _Chakma                 // Chakma is the set of Unicode characters in script Chakma.
        Cham                   = _Cham                   // Cham is the set of Unicode characters in script Cham.
        Cherokee               = _Cherokee               // Cherokee is the set of Unicode characters in script Cherokee.
        Common                 = _Common                 // Common is the set of Unicode characters in script Common.
        Coptic                 = _Coptic                 // Coptic is the set of Unicode characters in script Coptic.
        Cuneiform              = _Cuneiform              // Cuneiform is the set of Unicode characters in script Cuneiform.
        Cypriot                = _Cypriot                // Cypriot is the set of Unicode characters in script Cypriot.
        Cyrillic               = _Cyrillic               // Cyrillic is the set of Unicode characters in script Cyrillic.
        Deseret                = _Deseret                // Deseret is the set of Unicode characters in script Deseret.
        Devanagari             = _Devanagari             // Devanagari is the set of Unicode characters in script Devanagari.
        Egyptian_Hieroglyphs   = _Egyptian_Hieroglyphs   // Egyptian_Hieroglyphs is the set of Unicode characters in script Egyptian_Hieroglyphs.
        Ethiopic               = _Ethiopic               // Ethiopic is the set of Unicode characters in script Ethiopic.
        Georgian               = _Georgian               // Georgian is the set of Unicode characters in script Georgian.
        Glagolitic             = _Glagolitic             // Glagolitic is the set of Unicode characters in script Glagolitic.
        Gothic                 = _Gothic                 // Gothic is the set of Unicode characters in script Gothic.
        Greek                  = _Greek                  // Greek is the set of Unicode characters in script Greek.
        Gujarati               = _Gujarati               // Gujarati is the set of Unicode characters in script Gujarati.
        Gurmukhi               = _Gurmukhi               // Gurmukhi is the set of Unicode characters in script Gurmukhi.
        Han                    = _Han                    // Han is the set of Unicode characters in script Han.
        Hangul                 = _Hangul                 // Hangul is the set of Unicode characters in script Hangul.
        Hanunoo                = _Hanunoo                // Hanunoo is the set of Unicode characters in script Hanunoo.
        Hebrew                 = _Hebrew                 // Hebrew is the set of Unicode characters in script Hebrew.
        Hiragana               = _Hiragana               // Hiragana is the set of Unicode characters in script Hiragana.
        Imperial_Aramaic       = _Imperial_Aramaic       // Imperial_Aramaic is the set of Unicode characters in script Imperial_Aramaic.
        Inherited              = _Inherited              // Inherited is the set of Unicode characters in script Inherited.
        Inscriptional_Pahlavi  = _Inscriptional_Pahlavi  // Inscriptional_Pahlavi is the set of Unicode characters in script Inscriptional_Pahlavi.
        Inscriptional_Parthian = _Inscriptional_Parthian // Inscriptional_Parthian is the set of Unicode characters in script Inscriptional_Parthian.
        Javanese               = _Javanese               // Javanese is the set of Unicode characters in script Javanese.
        Kaithi                 = _Kaithi                 // Kaithi is the set of Unicode characters in script Kaithi.
        Kannada                = _Kannada                // Kannada is the set of Unicode characters in script Kannada.
        Katakana               = _Katakana               // Katakana is the set of Unicode characters in script Katakana.
        Kayah_Li               = _Kayah_Li               // Kayah_Li is the set of Unicode characters in script Kayah_Li.
        Kharoshthi             = _Kharoshthi             // Kharoshthi is the set of Unicode characters in script Kharoshthi.
        Khmer                  = _Khmer                  // Khmer is the set of Unicode characters in script Khmer.
        Lao                    = _Lao                    // Lao is the set of Unicode characters in script Lao.
        Latin                  = _Latin                  // Latin is the set of Unicode characters in script Latin.
        Lepcha                 = _Lepcha                 // Lepcha is the set of Unicode characters in script Lepcha.
        Limbu                  = _Limbu                  // Limbu is the set of Unicode characters in script Limbu.
        Linear_B               = _Linear_B               // Linear_B is the set of Unicode characters in script Linear_B.
        Lisu                   = _Lisu                   // Lisu is the set of Unicode characters in script Lisu.
        Lycian                 = _Lycian                 // Lycian is the set of Unicode characters in script Lycian.
        Lydian                 = _Lydian                 // Lydian is the set of Unicode characters in script Lydian.
        Malayalam              = _Malayalam              // Malayalam is the set of Unicode characters in script Malayalam.
        Mandaic                = _Mandaic                // Mandaic is the set of Unicode characters in script Mandaic.
        Meetei_Mayek           = _Meetei_Mayek           // Meetei_Mayek is the set of Unicode characters in script Meetei_Mayek.
        Meroitic_Cursive       = _Meroitic_Cursive       // Meroitic_Cursive is the set of Unicode characters in script Meroitic_Cursive.
        Meroitic_Hieroglyphs   = _Meroitic_Hieroglyphs   // Meroitic_Hieroglyphs is the set of Unicode characters in script Meroitic_Hieroglyphs.
        Miao                   = _Miao                   // Miao is the set of Unicode characters in script Miao.
        Mongolian              = _Mongolian              // Mongolian is the set of Unicode characters in script Mongolian.
        Myanmar                = _Myanmar                // Myanmar is the set of Unicode characters in script Myanmar.
        New_Tai_Lue            = _New_Tai_Lue            // New_Tai_Lue is the set of Unicode characters in script New_Tai_Lue.
        Nko                    = _Nko                    // Nko is the set of Unicode characters in script Nko.
        Ogham                  = _Ogham                  // Ogham is the set of Unicode characters in script Ogham.
        Ol_Chiki               = _Ol_Chiki               // Ol_Chiki is the set of Unicode characters in script Ol_Chiki.
        Old_Italic             = _Old_Italic             // Old_Italic is the set of Unicode characters in script Old_Italic.
        Old_Persian            = _Old_Persian            // Old_Persian is the set of Unicode characters in script Old_Persian.
        Old_South_Arabian      = _Old_South_Arabian      // Old_South_Arabian is the set of Unicode characters in script Old_South_Arabian.
        Old_Turkic             = _Old_Turkic             // Old_Turkic is the set of Unicode characters in script Old_Turkic.
        Oriya                  = _Oriya                  // Oriya is the set of Unicode characters in script Oriya.
        Osmanya                = _Osmanya                // Osmanya is the set of Unicode characters in script Osmanya.
        Phags_Pa               = _Phags_Pa               // Phags_Pa is the set of Unicode characters in script Phags_Pa.
        Phoenician             = _Phoenician             // Phoenician is the set of Unicode characters in script Phoenician.
        Rejang                 = _Rejang                 // Rejang is the set of Unicode characters in script Rejang.
        Runic                  = _Runic                  // Runic is the set of Unicode characters in script Runic.
        Samaritan              = _Samaritan              // Samaritan is the set of Unicode characters in script Samaritan.
        Saurashtra             = _Saurashtra             // Saurashtra is the set of Unicode characters in script Saurashtra.
        Sharada                = _Sharada                // Sharada is the set of Unicode characters in script Sharada.
        Shavian                = _Shavian                // Shavian is the set of Unicode characters in script Shavian.
        Sinhala                = _Sinhala                // Sinhala is the set of Unicode characters in script Sinhala.
        Sora_Sompeng           = _Sora_Sompeng           // Sora_Sompeng is the set of Unicode characters in script Sora_Sompeng.
        Sundanese              = _Sundanese              // Sundanese is the set of Unicode characters in script Sundanese.
        Syloti_Nagri           = _Syloti_Nagri           // Syloti_Nagri is the set of Unicode characters in script Syloti_Nagri.
        Syriac                 = _Syriac                 // Syriac is the set of Unicode characters in script Syriac.
        Tagalog                = _Tagalog                // Tagalog is the set of Unicode characters in script Tagalog.
        Tagbanwa               = _Tagbanwa               // Tagbanwa is the set of Unicode characters in script Tagbanwa.
        Tai_Le                 = _Tai_Le                 // Tai_Le is the set of Unicode characters in script Tai_Le.
        Tai_Tham               = _Tai_Tham               // Tai_Tham is the set of Unicode characters in script Tai_Tham.
        Tai_Viet               = _Tai_Viet               // Tai_Viet is the set of Unicode characters in script Tai_Viet.
        Takri                  = _Takri                  // Takri is the set of Unicode characters in script Takri.
        Tamil                  = _Tamil                  // Tamil is the set of Unicode characters in script Tamil.
        Telugu                 = _Telugu                 // Telugu is the set of Unicode characters in script Telugu.
        Thaana                 = _Thaana                 // Thaana is the set of Unicode characters in script Thaana.
        Thai                   = _Thai                   // Thai is the set of Unicode characters in script Thai.
        Tibetan                = _Tibetan                // Tibetan is the set of Unicode characters in script Tibetan.
        Tifinagh               = _Tifinagh               // Tifinagh is the set of Unicode characters in script Tifinagh.
        Ugaritic               = _Ugaritic               // Ugaritic is the set of Unicode characters in script Ugaritic.
        Vai                    = _Vai                    // Vai is the set of Unicode characters in script Vai.
        Yi                     = _Yi                     // Yi is the set of Unicode characters in script Yi.
)
```


These variables have type *RangeTable.
这些变量 是 *RangeTable 类型

```golang
var (
        ASCII_Hex_Digit                    = _ASCII_Hex_Digit                    // ASCII_Hex_Digit is the set of Unicode characters with property ASCII_Hex_Digit.
        Bidi_Control                       = _Bidi_Control                       // Bidi_Control is the set of Unicode characters with property Bidi_Control.
        Dash                               = _Dash                               // Dash is the set of Unicode characters with property Dash.
        Deprecated                         = _Deprecated                         // Deprecated is the set of Unicode characters with property Deprecated.
        Diacritic                          = _Diacritic                          // Diacritic is the set of Unicode characters with property Diacritic.
        Extender                           = _Extender                           // Extender is the set of Unicode characters with property Extender.
        Hex_Digit                          = _Hex_Digit                          // Hex_Digit is the set of Unicode characters with property Hex_Digit.
        Hyphen                             = _Hyphen                             // Hyphen is the set of Unicode characters with property Hyphen.
        IDS_Binary_Operator                = _IDS_Binary_Operator                // IDS_Binary_Operator is the set of Unicode characters with property IDS_Binary_Operator.
        IDS_Trinary_Operator               = _IDS_Trinary_Operator               // IDS_Trinary_Operator is the set of Unicode characters with property IDS_Trinary_Operator.
        Ideographic                        = _Ideographic                        // Ideographic is the set of Unicode characters with property Ideographic.
        Join_Control                       = _Join_Control                       // Join_Control is the set of Unicode characters with property Join_Control.
        Logical_Order_Exception            = _Logical_Order_Exception            // Logical_Order_Exception is the set of Unicode characters with property Logical_Order_Exception.
        Noncharacter_Code_Point            = _Noncharacter_Code_Point            // Noncharacter_Code_Point is the set of Unicode characters with property Noncharacter_Code_Point.
        Other_Alphabetic                   = _Other_Alphabetic                   // Other_Alphabetic is the set of Unicode characters with property Other_Alphabetic.
        Other_Default_Ignorable_Code_Point = _Other_Default_Ignorable_Code_Point // Other_Default_Ignorable_Code_Point is the set of Unicode characters with property Other_Default_Ignorable_Code_Point.
        Other_Grapheme_Extend              = _Other_Grapheme_Extend              // Other_Grapheme_Extend is the set of Unicode characters with property Other_Grapheme_Extend.
        Other_ID_Continue                  = _Other_ID_Continue                  // Other_ID_Continue is the set of Unicode characters with property Other_ID_Continue.
        Other_ID_Start                     = _Other_ID_Start                     // Other_ID_Start is the set of Unicode characters with property Other_ID_Start.
        Other_Lowercase                    = _Other_Lowercase                    // Other_Lowercase is the set of Unicode characters with property Other_Lowercase.
        Other_Math                         = _Other_Math                         // Other_Math is the set of Unicode characters with property Other_Math.
        Other_Uppercase                    = _Other_Uppercase                    // Other_Uppercase is the set of Unicode characters with property Other_Uppercase.
        Pattern_Syntax                     = _Pattern_Syntax                     // Pattern_Syntax is the set of Unicode characters with property Pattern_Syntax.
        Pattern_White_Space                = _Pattern_White_Space                // Pattern_White_Space is the set of Unicode characters with property Pattern_White_Space.
        Quotation_Mark                     = _Quotation_Mark                     // Quotation_Mark is the set of Unicode characters with property Quotation_Mark.
        Radical                            = _Radical                            // Radical is the set of Unicode characters with property Radical.
        STerm                              = _STerm                              // STerm is the set of Unicode characters with property STerm.
        Soft_Dotted                        = _Soft_Dotted                        // Soft_Dotted is the set of Unicode characters with property Soft_Dotted.
        Terminal_Punctuation               = _Terminal_Punctuation               // Terminal_Punctuation is the set of Unicode characters with property Terminal_Punctuation.
        Unified_Ideograph                  = _Unified_Ideograph                  // Unified_Ideograph is the set of Unicode characters with property Unified_Ideograph.
        Variation_Selector                 = _Variation_Selector                 // Variation_Selector is the set of Unicode characters with property Variation_Selector.
        White_Space                        = _White_Space                        // White_Space is the set of Unicode characters with property White_Space.
)
```
These variables have type *RangeTable.
这些变量有  *RangeTable类型


```golang
var CaseRanges = _CaseRanges
```


CaseRanges is the table describing case mappings for all letters with non-self mappings.
CaseRanges所有字母的表描述情况下映射与异己分子的映射

```golang
var Categories = map[string]*RangeTable{
        "C":  C,
        "Cc": Cc,
        "Cf": Cf,
        "Co": Co,
        "Cs": Cs,
        "L":  L,
        "Ll": Ll,
        "Lm": Lm,
        "Lo": Lo,
        "Lt": Lt,
        "Lu": Lu,
        "M":  M,
        "Mc": Mc,
        "Me": Me,
        "Mn": Mn,
        "N":  N,
        "Nd": Nd,
        "Nl": Nl,
        "No": No,
        "P":  P,
        "Pc": Pc,
        "Pd": Pd,
        "Pe": Pe,
        "Pf": Pf,
        "Pi": Pi,
        "Po": Po,
        "Ps": Ps,
        "S":  S,
        "Sc": Sc,
        "Sk": Sk,
        "Sm": Sm,
        "So": So,
        "Z":  Z,
        "Zl": Zl,
        "Zp": Zp,
        "Zs": Zs,
}

```

Categories is the set of Unicode category tables.
Categories是Unicode的组分类表。

```golang
var FoldCategory = map[string]*RangeTable{
        "Common":    foldCommon,
        "Greek":     foldGreek,
        "Inherited": foldInherited,
        "L":         foldL,
        "Ll":        foldLl,
        "Lt":        foldLt,
        "Lu":        foldLu,
        "M":         foldM,
        "Mn":        foldMn,
}

```

FoldCategory maps a category name to a table of code points outside the category that are equivalent under simple case folding to code points inside the category. 
If there is no entry for a category name, there are no such points.
FoldCategory 映射一个类别名称 到 类别外的 代码点 的表, 该类别 相当于   在简单的情况下，折叠的类别里面的代码点。
如果这 没有 输入 类别名称,  没有这样的点



```golang
var FoldScript = map[string]*RangeTable{}
```

FoldScript maps a script name to a table of code points outside the script that are equivalent under simple case folding to code points inside the script. 
If there is no entry for a script name, there are no such points.
FoldScript 映射一个 脚本名 到 脚本外的 代码点表 , 相当于 在简单的情况下，折叠在脚本代码点。
如果这 没有 输入 类别名称,  没有这样的点



```golang
var GraphicRanges = []*RangeTable{
        L, M, N, P, S, Zs,
}
```


GraphicRanges defines the set of graphic characters according to Unicode.
GraphicRanges 根据Unicode定义图形字符集。

```golang
var PrintRanges = []*RangeTable{
        L, M, N, P, S,
}
```
PrintRanges defines the set of printable characters according to Go. ASCII space, U+0020, is handled separately.
PrintRanges 根据GO定义一组可打印字符. ASCII space, U+0020, 单独处理


```golang
var Properties = map[string]*RangeTable{
        "ASCII_Hex_Digit":                    ASCII_Hex_Digit,
        "Bidi_Control":                       Bidi_Control,
        "Dash":                               Dash,
        "Deprecated":                         Deprecated,
        "Diacritic":                          Diacritic,
        "Extender":                           Extender,
        "Hex_Digit":                          Hex_Digit,
        "Hyphen":                             Hyphen,
        "IDS_Binary_Operator":                IDS_Binary_Operator,
        "IDS_Trinary_Operator":               IDS_Trinary_Operator,
        "Ideographic":                        Ideographic,
        "Join_Control":                       Join_Control,
        "Logical_Order_Exception":            Logical_Order_Exception,
        "Noncharacter_Code_Point":            Noncharacter_Code_Point,
        "Other_Alphabetic":                   Other_Alphabetic,
        "Other_Default_Ignorable_Code_Point": Other_Default_Ignorable_Code_Point,
        "Other_Grapheme_Extend":              Other_Grapheme_Extend,
        "Other_ID_Continue":                  Other_ID_Continue,
        "Other_ID_Start":                     Other_ID_Start,
        "Other_Lowercase":                    Other_Lowercase,
        "Other_Math":                         Other_Math,
        "Other_Uppercase":                    Other_Uppercase,
        "Pattern_Syntax":                     Pattern_Syntax,
        "Pattern_White_Space":                Pattern_White_Space,
        "Quotation_Mark":                     Quotation_Mark,
        "Radical":                            Radical,
        "STerm":                              STerm,
        "Soft_Dotted":                        Soft_Dotted,
        "Terminal_Punctuation":               Terminal_Punctuation,
        "Unified_Ideograph":                  Unified_Ideograph,
        "Variation_Selector":                 Variation_Selector,
        "White_Space":                        White_Space,
}
```

Properties is the set of Unicode property tables.
Properties 是 Unicode的属性表。


```golang
var Scripts = map[string]*RangeTable{
        "Arabic":                 Arabic,
        "Armenian":               Armenian,
        "Avestan":                Avestan,
        "Balinese":               Balinese,
        "Bamum":                  Bamum,
        "Batak":                  Batak,
        "Bengali":                Bengali,
        "Bopomofo":               Bopomofo,
        "Brahmi":                 Brahmi,
        "Braille":                Braille,
        "Buginese":               Buginese,
        "Buhid":                  Buhid,
        "Canadian_Aboriginal":    Canadian_Aboriginal,
        "Carian":                 Carian,
        "Chakma":                 Chakma,
        "Cham":                   Cham,
        "Cherokee":               Cherokee,
        "Common":                 Common,
        "Coptic":                 Coptic,
        "Cuneiform":              Cuneiform,
        "Cypriot":                Cypriot,
        "Cyrillic":               Cyrillic,
        "Deseret":                Deseret,
        "Devanagari":             Devanagari,
        "Egyptian_Hieroglyphs":   Egyptian_Hieroglyphs,
        "Ethiopic":               Ethiopic,
        "Georgian":               Georgian,
        "Glagolitic":             Glagolitic,
        "Gothic":                 Gothic,
        "Greek":                  Greek,
        "Gujarati":               Gujarati,
        "Gurmukhi":               Gurmukhi,
        "Han":                    Han,
        "Hangul":                 Hangul,
        "Hanunoo":                Hanunoo,
        "Hebrew":                 Hebrew,
        "Hiragana":               Hiragana,
        "Imperial_Aramaic":       Imperial_Aramaic,
        "Inherited":              Inherited,
        "Inscriptional_Pahlavi":  Inscriptional_Pahlavi,
        "Inscriptional_Parthian": Inscriptional_Parthian,
        "Javanese":               Javanese,
        "Kaithi":                 Kaithi,
        "Kannada":                Kannada,
        "Katakana":               Katakana,
        "Kayah_Li":               Kayah_Li,
        "Kharoshthi":             Kharoshthi,
        "Khmer":                  Khmer,
        "Lao":                    Lao,
        "Latin":                  Latin,
        "Lepcha":                 Lepcha,
        "Limbu":                  Limbu,
        "Linear_B":               Linear_B,
        "Lisu":                   Lisu,
        "Lycian":                 Lycian,
        "Lydian":                 Lydian,
        "Malayalam":              Malayalam,
        "Mandaic":                Mandaic,
        "Meetei_Mayek":           Meetei_Mayek,
        "Meroitic_Cursive":       Meroitic_Cursive,
        "Meroitic_Hieroglyphs":   Meroitic_Hieroglyphs,
        "Miao":                   Miao,
        "Mongolian":              Mongolian,
        "Myanmar":                Myanmar,
        "New_Tai_Lue":            New_Tai_Lue,
        "Nko":                    Nko,
        "Ogham":                  Ogham,
        "Ol_Chiki":               Ol_Chiki,
        "Old_Italic":             Old_Italic,
        "Old_Persian":            Old_Persian,
        "Old_South_Arabian":      Old_South_Arabian,
        "Old_Turkic":             Old_Turkic,
        "Oriya":                  Oriya,
        "Osmanya":                Osmanya,
        "Phags_Pa":               Phags_Pa,
        "Phoenician":             Phoenician,
        "Rejang":                 Rejang,
        "Runic":                  Runic,
        "Samaritan":              Samaritan,
        "Saurashtra":             Saurashtra,
        "Sharada":                Sharada,
        "Shavian":                Shavian,
        "Sinhala":                Sinhala,
        "Sora_Sompeng":           Sora_Sompeng,
        "Sundanese":              Sundanese,
        "Syloti_Nagri":           Syloti_Nagri,
        "Syriac":                 Syriac,
        "Tagalog":                Tagalog,
        "Tagbanwa":               Tagbanwa,
        "Tai_Le":                 Tai_Le,
        "Tai_Tham":               Tai_Tham,
        "Tai_Viet":               Tai_Viet,
        "Takri":                  Takri,
        "Tamil":                  Tamil,
        "Telugu":                 Telugu,
        "Thaana":                 Thaana,
        "Thai":                   Thai,
        "Tibetan":                Tibetan,
        "Tifinagh":               Tifinagh,
        "Ugaritic":               Ugaritic,
        "Vai":                    Vai,
        "Yi":                     Yi,
}
```

Scripts is the set of Unicode script tables.
Scripts 是 Unicode 脚本表集


func In
```golang
func In(r rune, ranges ...*RangeTable) bool
```
In reports whether the rune is a member of one of the ranges.
报告 rune 是否是ranges里的一员



func Is
```golang
func Is(rangeTab *RangeTable, r rune) bool
```
Is reports whether the rune is in the specified table of ranges.



func IsControl
```golang
func IsControl(r rune) bool
```
IsControl reports whether the rune is a control character. The C (Other) Unicode category includes more code points such as surrogates; use Is(C, r) to test for them.
IsControl 报告 rune 是否是一个控制字符. C Unicode类别包含 更多代码点 例如 surrogates;  为他们 使用 Is(C, r) 测试.



func IsDigit
```golang
func IsDigit(r rune) bool
```
IsDigit reports whether the rune is a decimal digit.
IsDigit 报告 rune 是否是一个 十进制数字。



func IsGraphic
```golang
func IsGraphic(r rune) bool
```
IsGraphic reports whether the rune is defined as a Graphic by Unicode. 
Such characters include letters, marks, numbers, punctuation, symbols, and spaces, from categories L, M, N, P, S, Zs.
IsGraphic 报告 rune 是否 通过 Unicode 定义 Graphic.
例如 字符包含 字母,标志、数字、标点符号、空格,   从类别  L, M, N, P, S, Zs.




func IsLetter
```golang
func IsLetter(r rune) bool
```
IsLetter reports whether the rune is a letter (category L).
IsLetter 报告 rune是否是一个 字母 (category L).



func IsLower
```golang
func IsLower(r rune) bool
```
IsLower reports whether the rune is a lower case letter.
IsLower 报告 rune 是否是一个小写字母



func IsMark
```golang
func IsMark(r rune) bool
```
IsMark reports whether the rune is a mark character (category M).
IsMark 报告 rune 是否是一个标示 字符 (category M).



func IsNumber
```golang
func IsNumber(r rune) bool
```
IsNumber reports whether the rune is a number (category N).
IsNumber 报告 rune是否是一个数字(category N).



func IsOneOf
```golang
func IsOneOf(ranges []*RangeTable, r rune) bool
```
IsOneOf reports whether the rune is a member of one of the ranges. The function "In" provides a nicer signature and should be used in preference to IsOneOf.
IsOneOf 报告 rune 是否是 ranges里的一员. 函数"In" 提供更好的签名和应该优先于IsOneOf使用。



func IsPrint
```golang
func IsPrint(r rune) bool
```
IsPrint reports whether the rune is defined as printable by Go. 
Such characters include letters, marks, numbers, punctuation, symbols, and the ASCII space character, from categories L, M, N, P, S and the ASCII space character. 
This categorization is the same as IsGraphic except that the only spacing character is ASCII space, U+0020.
IsPrint 报告 rune是否 通过GO定义打印.
如 字符 包含  字母,标志、数字、标点、符号、和ASCII空格字符,  从类别 L, M, N, P, S  和ASCII 空格字符.
这个分类和IsGraphic 是一样的 除了  ASCII space, U+0020 空格字符.



func IsPunct
```golang
func IsPunct(r rune) bool
```
IsPunct reports whether the rune is a Unicode punctuation character (category P).
IsPunct 报告 rune 是否 是一个Unicode 标点字符



func IsSpace
```golang
func IsSpace(r rune) bool
```
IsSpace reports whether the rune is a space character as defined by Unicode's White Space property; in the Latin-1 space this is
IsSpace 报告 rune是否是一个 空格字符, 通过Unicode 的空格属性定义的. 在 Latin-1  是

```golang
'\t', '\n', '\v', '\f', '\r', ' ', U+0085 (NEL), U+00A0 (NBSP).
```

Other definitions of spacing characters are set by category Z and property Pattern_White_Space.
其他的空格字符 通过 类别Z和属性 Pattern_White_Space 设置.



func IsSymbol
```golang
func IsSymbol(r rune) bool
```
IsSymbol reports whether the rune is a symbolic character.
IsSymbol 报告 rune是否 是 一个符号字符



func IsTitle
```golang
func IsTitle(r rune) bool
```
IsTitle reports whether the rune is a title case letter.
IsTitle 报告rune是否是 首字母大写



func IsUpper
```golang
func IsUpper(r rune) bool
```
IsUpper reports whether the rune is an upper case letter.
IsUpper 报告 rune 是否是大写字母


func SimpleFold
```golang
func SimpleFold(r rune) rune
```
SimpleFold iterates over Unicode code points equivalent under the Unicode-defined simple case folding. 
Among the code points equivalent to rune (including rune itself), SimpleFold returns the smallest rune > r if one exists, or else the smallest rune >= 0.
SimpleFold 迭代 Unicode 代码点 相当于  在Unicode的定义简单的例子折叠。
其中，所述代码点相当于rune(包含rune本身 ),SimpleFold 如果存在 返回 最小 rune > r,或 不存在 最小 rune >= 0.

For example:

```golang
SimpleFold('A') = 'a'
SimpleFold('a') = 'A'

SimpleFold('K') = 'k'
SimpleFold('k') = '\u212A' (Kelvin symbol, K)
SimpleFold('\u212A') = 'K'

SimpleFold('1') = '1'
```


func To
```golang
func To(_case int, r rune) rune
```
To maps the rune to the specified case: UpperCase, LowerCase, or TitleCase.
映射 rune成指定情况:UpperCase, LowerCase, 或 TitleCase.



func ToLower
```golang
func ToLower(r rune) rune
```
ToLower maps the rune to lower case.
ToLower 映射rune 成小写



func ToTitle
```golang
func ToTitle(r rune) rune
```
ToTitle maps the rune to title case.
ToTitle 映射rune成首字母大写



func ToUpper
```golang
func ToUpper(r rune) rune
```
ToUpper maps the rune to upper case.
ToUpper 映射 rune成大写



type CaseRange
```golang
type CaseRange struct {
        Lo    uint32
        Hi    uint32
        Delta d
}
```
CaseRange represents a range of Unicode code points for simple (one code point to one code point) case conversion. 
The range runs from Lo to Hi inclusive, with a fixed stride of 1. 
Deltas are the number to add to the code point to reach the code point for a different case for that character. 
They may be negative. If zero, it means the character is in the corresponding case. 
There is a special case representing sequences of alternating corresponding Upper and Lower pairs. It appears with a fixed Delta of
CaseRange 表示一个 Unicode代码点 范围, 对于简单的(一个代码点到一个代码点)转换。
执行的范围从Lo 到 Hi 包括 , 固定 步 1.
Deltas 是 增加数量的代码点不同的情况下,字符的代码点。
可能是负的. 如果零 , 它意味着 字符在对应的情况.
有一个特殊情况表示 大写 和小写对 交替序列 . 它有固定的Delta

```golang
{UpperLower, UpperLower, UpperLower}
```

The constant UpperLower has an otherwise impossible delta value.
常量 UpperLower 否则 不可能是delta值


type Range16
```golang
type Range16 struct {
        Lo     uint16
        Hi     uint16
        Stride uint16
}
```
Range16 represents of a range of 16-bit Unicode code points. The range runs from Lo to Hi inclusive and has the specified stride.
Range16 表示一个 16位 Unicode 代码点范围. 范围从  Lo 到 Hi 包括 他们本身  和有指定的步伐。


type Range32
```golang
type Range32 struct {
        Lo     uint32
        Hi     uint32
        Stride uint32
}
```
Range32 represents of a range of Unicode code points and is used when one or more of the values will not fit in 16 bits. 
The range runs from Lo to Hi inclusive and has the specified stride. Lo and Hi must always be >= 1<<16.
Range32 表示一个 Unicode 代码点范围 和 在16位里当一个或多个值 不适合时 使用.
范围从Lo 到 Hi 包含他们本身 并且有指定的步. Lo 和  Hi 必须  >= 1<<16.


type RangeTable
```golang
type RangeTable struct {
        R16         []Range16
        R32         []Range32
        LatinOffset int // number of entries in R16 with Hi <= MaxLatin1
}
```
RangeTable defines a set of Unicode code points by listing the ranges of code points within the set. 
The ranges are listed in two slices to save space: a slice of 16-bit ranges and a slice of 32-bit ranges. 
The two slices must be in sorted order and non-overlapping. Also, R32 should contain only values >= 0x10000 (1<<16).
RangeTable 通过在 代码点范围里的集列表 定义 Unicode代码点集.
范围 的里表在两个slice 来包含空间:一个16位范围slice 和一个32位为范围slice.
两个slice必须按顺序排列 并且不能重复. 所以 R32 只能包含values >= 0x10000 (1<<16).




type SpecialCase
```golang
type SpecialCase []CaseRange
```
SpecialCase represents language-specific case mappings such as Turkish. Methods of SpecialCase customize (by overriding) the standard mappings.
SpecialCase 表示 特定于语言的映射,如土耳其.  SpecialCase方法 自定义(通过重写)标准的映射。

```golang
var AzeriCase SpecialCase = _TurkishCase
var TurkishCase SpecialCase = _TurkishCase
```



func (SpecialCase) ToLower
```golang
func (special SpecialCase) ToLower(r rune) rune
```
ToLower maps the rune to lower case giving priority to the special mapping.
ToLower 映射rune 成小写, 优先考虑特殊的映射  


func (SpecialCase) ToTitle
```golang
func (special SpecialCase) ToTitle(r rune) rune
```
ToTitle maps the rune to title case giving priority to the special mapping.
ToTitle 映射rune 成首字母大写, 优先考虑特殊的映射


func (SpecialCase) ToUpper
```golang
func (special SpecialCase) ToUpper(r rune) rune
```
ToUpper maps the rune to upper case giving priority to the special mapping.
ToUpper映射rune 成大写, 优先考虑特殊的映射


























	