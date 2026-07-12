System
├── System (root)
│   ├── Object, String, Int32, Int64, Double, Decimal, Boolean, Char
│   ├── DateTime, DateTimeOffset, TimeSpan, Guid
│   ├── Console
│   ├── Math
│   ├── Array, Enum, Nullable<T>
│   ├── Exception, ArgumentException, NullReferenceException, InvalidOperationException
│   ├── Convert
│   ├── Random
│   ├── Action<T>, Func<T>, Predicate<T>
│   ├── IDisposable, IComparable, IEquatable<T>
│   └── Environment, AppDomain
│
├── System.Collections
│   ├── ArrayList, Hashtable, Queue, Stack
│   └── IEnumerable, ICollection, IList, IDictionary
│
├── System.Collections.Generic
│   ├── List<T>, Dictionary<TKey,TValue>, HashSet<T>
│   ├── Queue<T>, Stack<T>, LinkedList<T>
│   ├── SortedList<T>, SortedDictionary<T>, SortedSet<T>
│   ├── IEnumerable<T>, ICollection<T>, IList<T>, IDictionary<TKey,TValue>
│   └── Comparer<T>, EqualityComparer<T>
│
├── System.Collections.Concurrent
│   ├── ConcurrentDictionary<TKey,TValue>
│   ├── ConcurrentQueue<T>, ConcurrentStack<T>
│   └── BlockingCollection<T>
│
├── System.Collections.ObjectModel
│   ├── Collection<T>
│   ├── ReadOnlyCollection<T>
│   └── ObservableCollection<T>
│
├── System.Linq
│   ├── Enumerable (Where, Select, OrderBy, GroupBy...)
│   ├── Queryable
│   ├── IQueryable<T>
│   └── ILookup<TKey,TElement>
│
├── System.Text
│   ├── StringBuilder
│   ├── Encoding (UTF8, ASCII, Unicode)
│   └── Rune
│
├── System.Text.RegularExpressions
│   ├── Regex
│   ├── Match, MatchCollection
│   └── Group, GroupCollection
│
├── System.Text.Json
│   ├── JsonSerializer
│   ├── JsonDocument, JsonElement
│   └── JsonNode, JsonObject, JsonArray (System.Text.Json.Nodes)
│
├── System.IO
│   ├── File, FileInfo, Directory, DirectoryInfo
│   ├── Stream, FileStream, MemoryStream
│   ├── StreamReader, StreamWriter
│   ├── BinaryReader, BinaryWriter
│   ├── Path
│   └── TextReader, TextWriter
│
├── System.IO.Compression
│   ├── ZipFile
│   ├── ZipArchive, ZipArchiveEntry
│   └── GZipStream, DeflateStream
│
├── System.Net
│   ├── WebClient (legacy), Dns, IPAddress
│   └── System.Net.Http
│       ├── HttpClient
│       ├── HttpRequestMessage, HttpResponseMessage
│       └── HttpContent
│
├── System.Net.Sockets
│   ├── Socket
│   ├── TcpClient, TcpListener
│   └── UdpClient
│
├── System.Net.Mail
│   ├── SmtpClient
│   ├── MailMessage
│   └── MailAddress
│
├── System.Threading
│   ├── Thread
│   ├── Mutex, Semaphore, SemaphoreSlim
│   ├── Monitor, ManualResetEvent, AutoResetEvent
│   ├── Interlocked
│   ├── CancellationToken, CancellationTokenSource
│   └── ThreadPool
│
├── System.Threading.Tasks
│   ├── Task, Task<TResult>
│   ├── TaskFactory, TaskScheduler
│   ├── ValueTask<T>
│   └── Parallel
│
├── System.Data
│   ├── DataSet, DataTable, DataRow, DataColumn
│   ├── DataView
│   └── System.Data.Common
│       ├── DbConnection, DbCommand, DbDataReader
│       └── DbTransaction
│
├── System.Data.SqlClient / Microsoft.Data.SqlClient
│   ├── SqlConnection
│   ├── SqlCommand
│   ├── SqlDataReader
│   └── SqlTransaction
│
├── System.Reflection
│   ├── Assembly
│   ├── Type, MemberInfo, MethodInfo, PropertyInfo, FieldInfo
│   └── Attribute
│
├── System.Diagnostics
│   ├── Process
│   ├── Stopwatch
│   ├── Debug, Trace
│   └── EventLog
│
├── System.Globalization
│   ├── CultureInfo
│   ├── NumberFormatInfo
│   └── DateTimeFormatInfo
│
├── System.Runtime.Serialization
│   ├── DataContractSerializer
│   └── ISerializable
│
├── System.Xml
│   ├── XmlDocument, XmlElement, XmlNode
│   ├── XmlReader, XmlWriter
│   └── System.Xml.Linq
│       ├── XDocument, XElement, XAttribute
│
├── System.ComponentModel
│   ├── INotifyPropertyChanged
│   ├── BackgroundWorker
│   └── System.ComponentModel.DataAnnotations
│       ├── Required, StringLength, Range (attributes)
│
├── System.Security
│   ├── System.Security.Cryptography
│   │   ├── Aes, RSA, SHA256, MD5
│   │   └── HMACSHA256
│   └── System.Security.Principal
│       └── WindowsIdentity, IPrincipal
│
└── System.Reflection.Emit (dynamic code generation)
    ├── AssemblyBuilder
    └── TypeBuilder