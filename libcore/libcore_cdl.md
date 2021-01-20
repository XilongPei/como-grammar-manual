
28 directories, 226 files
参考样式： https://docs.microsoft.com/zh-cn/dotnet/core/introduction


# como



## core



### Errors.cdl




### Exceptions.cdl

### IAnnotation.cdl
### IAppendable.cdl
### IArray.cdl
### IArrayHolder.cdl
### IAutoCloseable.cdl
### IBoolean.cdl



### IByte.cdl
como.core

##### 接口(interface )

IByte

##### 常量(Consts)

- const Byte MIN_VALUE = -128;

A constant holding the minimum value a {@code byte} can  have, -2<sup>7</sup>.

- const Byte MAX_VALUE = 127;

A constant holding the maximum value a {@code byte} can have, 2<sup>7</sup>-1.

- const Integer SIZE = 8;

##### 方法总汇(Method Summary)


```idl
ByteValue([out] Byte& value);
CompareTo([in] IInterface* other, [out] Integer& result);
DoubleValue([out] Double& value);
FloatValue([out] Float& value);
GetValue([out] Byte& value);
IntegerValue([out] Integer& value);
LongValue([out] Long& value);
ShortValue([out] Short& value);
```

##### 方法详解(Method Detail)

- ByteValue([out] Byte& value);

  **参数(Parameters):**

  [out] Byte& value



### IChar.cdl





### ICharSequence.cdl
como.core

##### 接口(interface )

ICharSequence

##### 常量(Consts)


##### 方法总汇(Method Summary)


```idl
GetCharAt([in] Integer index, [out] Char& c);
GetLength([out] Integer& number);
SubSequence([in] Integer start, [in] Integer end, [out] ICharSequence&& subcsq);
ToString([out] String& str);
```

##### 方法详解(Method Detail)

- GetCharAt([in] Integer index, [out] Char& c);

     **参数(Parameters):**

     1. [in] Integer index

     2. [out] Char& c



   ### ICloneable.cdl

### IComparable.cdl
### IDouble.cdl
### IFloat.cdl
### IInteger.cdl
### IIterable.cdl
### ILong.cdl
### INumber.cdl
### IReadable.cdl
### IRunnable.cdl
### IRuntime.cdl
### ISecurityManager.cdl
### IShort.cdl
### IStackTrace.cdl
### IStackTraceElement.cdl
### IString.cdl
### IStringBuffer.cdl
### IStringBuilder.cdl
### ISynchronize.cdl
### ISystem.cdl
### IThread.cdl
### IThreadGroup.cdl

### IThreadLocal.cdl

## core.cdl
## core2.cdl
## core3.cdl

## io
### Errors.cdl
### Exceptions.cdl
### IBuffer.cdl
### IBufferedWriter.cdl
### IByteArrayOutputStream.cdl
### IByteBuffer.cdl
### IByteOrder.cdl
### ICharBuffer.cdl
### ICloseable.cdl
### IDataInput.cdl
### IDoubleBuffer.cdl
### IFile.cdl
### IFileDescriptor.cdl
### IFileFilter.cdl
### IFileInputStream.cdl
### IFileOutputStream.cdl
### IFilenameFilter.cdl
### IFloatBuffer.cdl
### IFlushable.cdl
### IInputStream.cdl
### IIntegerBuffer.cdl
### IInterruptible.cdl
### ILongBuffer.cdl
### IMappedByteBuffer.cdl
### IObjectInput.cdl
### IObjectInputStream.cdl
### IOutputStream.cdl
### IOutputStreamWriter.cdl
### IPrintStream.cdl
### IPrintWriter.cdl
### IReader.cdl
### ISerializable.cdl
### IShortBuffer.cdl
### IStringWriter.cdl
### IWriter.cdl
### channels
#### IDirectBuffer.cdl
#### IFileChannel.cdl
### charset
#### ICharset.cdl
#### ICharsetDecoder.cdl
#### ICharsetEncoder.cdl
#### ICoderResult.cdl
#### ICodingErrorAction.cdl
#### IStreamEncoder.cdl
#### spi
##### ICharsetProvider.cdl
## io.cdl
## math
### IBigDecimal.cdl
### IBigInteger.cdl
### IMathContext.cdl
### RoundingMode.cdl
## math.cdl
## misc
### IFDBigInteger.cdl
### IFloatingDecimal.cdl
### IFormattedFloatingDecimal.cdl
## misc.cdl
## net
### IDatagramPacket.cdl
### IDatagramSocket.cdl
### IInetAddress.cdl
### IInetSocketAddress.cdl
### ISocket.cdl
### ISocketAddress.cdl
### IURI.cdl
### IURL.cdl
## net.cdl
## security
### IGuard.cdl
### IPermission.cdl
### IPermissionCollection.cdl
### IPrivilegedAction.cdl
### IProvider.cdl
### ISecureRandom.cdl
### ISecureRandomSpi.cdl
### cca
#### IInstanceFactory.cdl
#### IProviderList.cdl
## security.cdl
## text
### Exceptions.cdl
### IAttributedCharacterIterator.cdl
### IAttributedString.cdl
### ICharacterIterator.cdl
### IChoiceFormat.cdl
### IDateFormat.cdl
### IDateFormatSymbols.cdl
### IDecimalFormat.cdl
### IDecimalFormatSymbols.cdl
### IFieldPosition.cdl
### IFormat.cdl
### IMessageFormat.cdl
### INumberFormat.cdl
### IParsePosition.cdl
### ISimpleDateFormat.cdl
## text.cdl
## util
### Exceptions.cdl
### IArrayList.cdl
### ICalendar.cdl
### ICollection.cdl
### IComparator.cdl
### ICurrency.cdl
### IDate.cdl
### IDeque.cdl
### IDictionary.cdl
### IEnumeration.cdl
### IFormattable.cdl
### IFormatter.cdl
### IGregorianCalendar.cdl
### IHashMap.cdl
### IHashSet.cdl
### IHashtable.cdl
### IIterator.cdl
### IJapaneseImperialCalendar.cdl
### ILinkedHashMap.cdl
### ILinkedHashSet.cdl
### ILinkedList.cdl
### IList.cdl
### IListIterator.cdl
### ILocale.cdl
### IMap.cdl
### INavigableMap.cdl
### INavigableSet.cdl
### IProperties.cdl
### IQueue.cdl
### IRandom.cdl
### IRandomAccess.cdl
### IServiceLoader.cdl
### ISet.cdl
### ISimpleTimeZone.cdl
### ISortedMap.cdl
### ISortedSet.cdl
### IStringTokenizer.cdl
### ITimeZone.cdl
### ITimeZoneGetter.cdl
### ITreeMap.cdl
### ITreeSet.cdl
### IVector.cdl
### calendar
#### IBaseCalendar.cdl
#### ICalendarDate.cdl
#### ICalendarSystem.cdl
#### IEra.cdl
#### IGregorian.cdl
#### IJulianCalendar.cdl
#### ILocalGregorianCalendar.cdl
### concurrent
#### IConcurrentHashMap.cdl
#### IConcurrentLinkedQueue.cdl
#### IConcurrentMap.cdl
#### IThreadLocalRandom.cdl
#### atomic
##### IAtomicBoolean.cdl
##### IAtomicInteger.cdl
##### IAtomicLong.cdl
### locale
#### ILanguageTag.cdl
### regex
#### Exceptions.cdl
#### IMatchResult.cdl
#### IMatcher.cdl
#### IPattern.cdl

## util.cdl





# comort

## system
### IBlockGuard.cdl
### ICloseGuard.cdl
### ISocketTagger.cdl
## system.cdl





# jing

## icu
### text
#### ITimeZoneNames.cdl
## icu.cdl
## system
### Exceptions.cdl
### IStructAddrinfo.cdl
### IStructCapUserData.cdl
### IStructCapUserHeader.cdl
### IStructFlock.cdl
### IStructGroupReq.cdl
### IStructGroupSourceReq.cdl
### IStructIfaddrs.cdl
### IStructLinger.cdl
### IStructPasswd.cdl
### IStructPollfd.cdl
### IStructStat.cdl
### IStructStatVfs.cdl
### IStructTimeval.cdl
### IStructUcred.cdl
### IStructUtsname.cdl
### system.cdl





# libcore

## icu
### IICU.cdl
### ILocaleData.cdl
### ITimeZoneNames.cdl
## icu.cdl
## io
### IBufferIterator.cdl
### IIoBridge.cdl
### ILibcore.cdl
### IMemoryMappedFile.cdl
### IOs.cdl
## io.cdl
## util
### IBasicLruCache.cdl
### IZoneInfo.cdl
### IZoneInfoDB.cdl




# util.cdl
