包地址：http://golang.org/pkg/database/sql/driver/

```golang
Package driver defines interfaces to be implemented by database drivers as used by package sql.
Most code should use package sql.
driver包 使用sql包定义实现 数据引擎的接口
大部分的代码都用sql包

```

Variables
```golang
  var Bool boolType
    Bool is a ValueConverter that converts input values to bools.
    是一个值转换，转换输入的值成bools
    
```golang
    The conversion rules are:
    - booleans are returned unchanged
    - for integer types, 
	     1 is true
	     0 is false,
      other integers are an error
    - for strings and []byte, same rules as strconv.ParseBool
    - all other types are an error
		转换规则：
		布尔值返回不变
		-整型类型
		1是true
		0是false
		其他整型是错误
		string和[]byte 和strconv.ParseBool 的规则一样
		其他类型是错误
```

```golang
  var DefaultParameterConverter defaultConverter
```
   DefaultParameterConverter is the default implementation of ValueConverter that's used when a Stmt doesn't implement ColumnConverter.
   DefaultParameterConverter returns the given value directly if IsValue(value). 
   Otherwise integer type are converted to int64, floats to float64, and strings to []byte. Other types are an error.
    当一个Stmt 没有实现ColumnConverter 默认实现ValueConverter。
    如果IsValue(value) 返回给定的值。
    其他的整型转换成int64,floats 转换成float64，strings 转换成 []byte。其他类型是错误。

```golang    
  var ErrBadConn = errors.New("driver: bad connection")
```
    ErrBadConn should be returned by a driver to signal to the sql package that a driver.Conn 
    is in a bad state (such as the server having earlier closed the connection) and the sql package should retry on a new connection.
    To prevent duplicate operations, ErrBadConn should NOT be returned if there's a possibility that the database server might have performed the operation. 
    	Even if the server sends back an error, you shouldn't return ErrBadConn.
    当driver传递信号到sql包 会返回driver.Conn 的 错误的状态 （例如太早关闭连接），sql包会重试一个新连接。
    为了防止重复操作，如果数据库服务器可能已经执行操作，ErrBadConn 不应该返回。甚至如果服务器发送返回一个错误，你不应该返回ErrBadConn

```golang  
  var ErrSkip = errors.New("driver: skip fast-path; continue as if unimplemented")
```  
    ErrSkip may be returned by some optional interfaces' methods to indicate at runtime that the fast path is unavailable 
    	and the sql package should continue as if the optional interface was not implemented. 
    ErrSkip is only supported where explicitly documented.
    ErrSkip 可能会在 一些 可选接口方法运行时的最快路径无法使用的时候返回。如果接收可选接口没有实现时sql包应该继续。
    ErrSkip只支持明确的记载。 

```golang    
  var Int32 int32Type
```
    Int32 is a ValueConverter that converts input values to int64, respecting the limits of an int32 value.
    Int32是一个 ValueConverter 输入的值被转换成int64，遵循int32的值限制。
    
```golang    
  var ResultNoRows noRows
```  
    ResultNoRows is a pre-defined Result for drivers to return when a DDL command (such as a CREATE TABLE) succeeds. 
    It returns an error for both LastInsertId and RowsAffected.
    ResultNoRows  当一个DDL （比如创建一个表）命令成功 返回一个预定义的引擎结果。不管是LastInsertId 还是RowsAffected 会返回错误。

```golang  
  var String stringType
```  
    String is a ValueConverter that converts its input to a string. 
    If the value is already a string or []byte, it's unchanged. 
    If the value is of another type, conversion to string is done with fmt.Sprintf("%v", v).
     值转化成string。如果值已经是string或者[]byte 值是不变的 。如果值是其他类型使用fmt.Sprintf("%v", v) 转换string
```

func IsScanValue
```golang
	func IsScanValue(v interface{}) bool
	  IsScanValue reports whether v is a valid Value scan type. Unlike IsValue, IsScanValue does not permit the string type.
	  返回 v是否一个有效的scan 类型的值。不像IsValue，IsScanValue不允许string类型。
```

func IsValue
```golang
	func IsValue(v interface{}) bool
	  IsValue reports whether v is a valid Value parameter type. Unlike IsScanValue, IsValue permits the string type.
	   返回 v是否一个有效的参数 类型的值。不像IsScanValue，IsValue允许string类型
```


type ColumnConverter
```golang
  type ColumnConverter interface {
        // ColumnConverter returns a ValueConverter for the provided
        // column index.  If the type of a specific column isn't known
        // or shouldn't be handled specially, DefaultValueConverter
        // can be returned.
        ColumnConverter(idx int) ValueConverter
  }
  ColumnConverter may be optionally implemented by Stmt if the the statement is aware of its own columns' types and can convert from any type to a driver Value.
  ColumnConverter 如果列类型可以从任何driver 值转换,可以通过Stmt有选择的实现
```

type Conn
```golang
  type Conn interface {
        // Prepare returns a prepared statement, bound to this connection.
        Prepare(query string) (Stmt, error)

        // Close invalidates and potentially stops any current
        // prepared statements and transactions, marking this
        // connection as no longer in use.
        //
        // Because the sql package maintains a free pool of
        // connections and only calls Close when there's a surplus of
        // idle connections, it shouldn't be necessary for drivers to
        // do their own connection caching.
        Close() error

        // Begin starts and returns a new transaction.
        Begin() (Tx, error)
  }
  Conn is a connection to a database. It is not used concurrently by multiple goroutines.
  Conn is assumed to be stateful.
  连接数据库。它没有使用多goroutines并发
  Conn是假定有状态的
```  
type Driver
```golang
  type Driver interface {
        // Open returns a new connection to the database.
        // The name is a string in a driver-specific format.
        //
        // Open may return a cached connection (one previously
        // closed), but doing so is unnecessary; the sql package
        // maintains a pool of idle connections for efficient re-use.
        //
        // The returned connection is only used by one goroutine at a
        // time.
        Open(name string) (Conn, error)
  }
  Driver is the interface that must be implemented by a database driver.
  Driver 是 必须实现的 数据库引擎接口
```
  
type Execer
```golang
  type Execer interface {
        Exec(query string, args []Value) (Result, error)
  }
  Execer is an optional interface that may be implemented by a Conn.
  If a Conn does not implement Execer, the sql package's DB.Exec will first prepare a query, execute the statement, and then close the statement.
  Exec may return ErrSkip.
  Execer 是一个使用Conn实现的可选接口
  如果Conn没有实现Execer，sql包DB.Exec 将会首先准备一个查询，执行语句，然后关闭语句。
  Exec 可能会返回ErrSkip
```

type NotNull
```golang
  type NotNull struct {
        Converter ValueConverter
  }
  NotNull is a type that implements ValueConverter by disallowing nil values but otherwise delegating to another ValueConverter.
  NotNull 是一个不允许为nil 的ValueConverter 实现的类型， 否则会委托到另外的ValueConverter
```

```golang
  func (n NotNull) ConvertValue(v interface{}) (Value, error)
```
  
type Null
```golang
  type Null struct {
        Converter ValueConverter
  }
  Null is a type that implements ValueConverter by allowing nil values but otherwise delegating to another ValueConverter.
  Null 是一个允许为nil 的ValueConverter 实现的类型， 否则会委托到另外的ValueConverter
```

```golang  
  func (n Null) ConvertValue(v interface{}) (Value, error)
```

type Queryer
```golang
  type Queryer interface {
        Query(query string, args []Value) (Rows, error)
  }
  Queryer is an optional interface that may be implemented by a Conn.
  If a Conn does not implement Queryer, the sql package's DB.Query will first prepare a query, execute the statement, and then close the statement.
  Query may return ErrSkip.
  Queryer 是一个用Conn实现的可选接口
   如果Conn 没有实现Queryer，sql包DB.Query 将会首先准备一个查询，执行语句，然后关闭语句。
  Query 可能会返回ErrSkip
```
  
type Result
```golang
  type Result interface {
        // LastInsertId returns the database's auto-generated ID
        // after, for example, an INSERT into a table with primary
        // key.
        LastInsertId() (int64, error)

        // RowsAffected returns the number of rows affected by the
        // query.
        RowsAffected() (int64, error)
  }
  Result is the result of a query execution.
  是一个执行查询的结果
```

type Rows
```golang
  type Rows interface {
        // Columns returns the names of the columns. The number of
        // columns of the result is inferred from the length of the
        // slice.  If a particular column name isn't known, an empty
        // string should be returned for that entry.
        Columns() []string

        // Close closes the rows iterator.
        Close() error

        // Next is called to populate the next row of data into
        // the provided slice. The provided slice will be the same
        // size as the Columns() are wide.
        //
        // The dest slice may be populated only with
        // a driver Value type, but excluding string.
        // All string values must be converted to []byte.
        //
        // Next should return io.EOF when there are no more rows.
        Next(dest []Value) error
  }
  Rows is an iterator over an executed query's results.
  迭代一个查询结果
```

type RowsAffected
```golang
  RowsAffected implements Result for an INSERT or UPDATE operation which mutates a number of rows.
  实现INSERT或者UPDATE操作的受影响的行数。
```

```golang
  func (RowsAffected) LastInsertId() (int64, error)
```

``golang  
  func (v RowsAffected) RowsAffected() (int64, error)
```

type Stmt
```golang
  type Stmt interface {
        // Close closes the statement.
        //
        // As of Go 1.1, a Stmt will not be closed if it's in use
        // by any queries.
        Close() error

        // NumInput returns the number of placeholder parameters.
        //
        // If NumInput returns >= 0, the sql package will sanity check
        // argument counts from callers and return errors to the caller
        // before the statement's Exec or Query methods are called.
        //
        // NumInput may also return -1, if the driver doesn't know
        // its number of placeholders. In that case, the sql package
        // will not sanity check Exec or Query argument counts.
        NumInput() int

        // Exec executes a query that doesn't return rows, such
        // as an INSERT or UPDATE.
        Exec(args []Value) (Result, error)

        // Exec executes a query that may return rows, such as a
        // SELECT.
        Query(args []Value) (Rows, error)
  }
  Stmt is a prepared statement. It is bound to a Conn and not used by multiple goroutines concurrently.
  Stmt 准备 一个语句。是Conn的界线， 没有同时使用多个goroutines。
```

type Tx
```golang
  type Tx interface {
        Commit() error
        Rollback() error
  }
  Tx is a transaction.
  Tx是一个事务
```

type Value
```golang
```golang
  type Value interface{}
```
    Value is a value that drivers must be able to handle. It is either nil or an instance of one of these types:
    Value必须是引擎能处理的值。它是nil 或者以下这些类型的实例：
```golang
  int64
  float64
  bool
  []byte
  string   [*] everywhere except from Rows.Next.
  time.Time
```
```

type ValueConverter
```golang
  type ValueConverter interface {
        // ConvertValue converts a value to a driver Value.
        ConvertValue(v interface{}) (Value, error)
  }
  ValueConverter is the interface providing the ConvertValue method.
  Various implementations of ValueConverter are provided by the driver package to provide consistent implementations of conversions between drivers. The ValueConverters have several uses:
  * converting from the Value types as provided by the sql package
  into a database table's specific column type and making sure it
  fits, such as making sure a particular int64 fits in a
  table's uint16 column.

  * converting a value as given from the database into one of the
    driver Value types.
  
  * by the sql package, for converting from a driver's Value type
    to a user's type in a scan.
    
  ValueConverter 是 提供ConvertValue方法的接口
  Various 实现driver包 在引擎之间转换 实现ValueConverter 。ValueConverters 有这些用法：
  sql包转换值类型成数据表的特定列类型，确保被填充。就如确保特定的int64填充进表的uint16列。
  转换给定的数据库引擎值类型。
  使用sql包 给使用者在扫描的类型从 driver's 值类型 转换。
```

type Valuer
```golang
  type Valuer interface {
        // Value returns a driver Value.
        Value() (Value, error)
  }
  Valuer is the interface providing the Value method.
  Valuer 提供Value方法的接口
  Types implementing Valuer interface are able to convert themselves to a driver Value.
  Types 实现Valuer接口 把它们本身转换成引擎值
```
