---
emoji: ๐
title: lombok์ @Builder๋ฅผ ์ถ๊ฐํ๋๋ ์ญ์จ์์ ์๋ฌ๊ฐ?
date: '2022-08-14 22:32:01'
author: mskangg
tags: java spring lombok jackson
categories: ํธ๋ฌ๋ธ์ํ
---  

![main](https://images.unsplash.com/photo-1613834927301-1c96a302e074?ixlib=rb-1.2.1&q=80&cs=tinysrgb&fm=jpg&crop=entropy)

## ๋ฌธ์  ๋ฐ์

ํ๊ฐ๋ก์ด ์คํ ๊ฐ๋ฐํ๊ฒฝ์ api ์๋ต ์๋ฌ๊ฐ ๋๋ค๊ณ  ๋ฌธ์๊ฐ ๋ค์ด์์ต๋๋ค.  
ES์ ์๋ฌ ๋ก๊ทธ๋ฅผ ํ์ธํด๋ณด๋ ์ด๋ฌํ ์๋ฌ ๋ก๊ทธ๊ฐ.. ์ค์ค..  
(์ค์  ํ๊ฒฝ์ ํฌ์คํํ  ์ ์์ด ์์  ์ฝ๋๋ฅผ ์์ฑํ์ฌ ์๋ฌ๋ฅผ ์ ๋ํจ)  

```bash
Resolved [org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Cannot construct instance of `com.example.lomboktest.SampleDto` (although at least one Creator exists): cannot deserialize from Object value (no delegate- or property-based Creator); 
nested exception is com.fasterxml.jackson.databind.exc.MismatchedInputException: Cannot construct instance of `com.example.lomboktest.SampleDto` (although at least one Creator exists): cannot deserialize from Object value (no delegate- or property-based Creator)<EOL> at [Source: (org.springframework.util.StreamUtils$NonClosingInputStream); line: 2, column: 5]]
```

๋ฌธ์ ๊ฐ ๋ฐ์ํ dto๋ฅผ ํ์ธํด๋ด์๋ค.  

```java
@Getter
public class SampleDto {
    @Builder.Default
    private String test = "tesT";

    @Builder
    public SampleDto(String test) {
        this.test = test;
    }
}
```

์.. ์์ฑ์๋ฅผ ์ ์ธํ๊ณ  ๋น๋๋ฅผ ์ ์ธํ์๋๋ฐ ๋ฌด์์ด ๋ฌธ์ ์ผ๊น?  
์๋ฌ ๋ก๊ทธ๋ฅผ ์์ธํ ํ์ธํด๋ด์๋ค.  

```java
JSON parse error:
Cannot construct instance of `com.example.lomboktest.SampleDto` 
(although at least one Creator exists): 
cannot deserialize from Object value (no delegate- or property-based Creator);
```

์ง์ญํ์๋ฉด,  JSON ํ์ฑ ์๋ฌ SampleDto๋ฅผ ์์ฑํ  ์ ์๋ค. (์ต์ ํ๊ฐ์ ์์ฑ์๊ฐ ์กด์ฌํ์ง๋ง)  
๊ฐ์ฒด๊ฐ์ผ๋ก๋ถํฐ ์ญ์ง๋ ฌํ๋ฅผ ํ  ์ ์๋ค. (delegate ๋๋ property ๊ธฐ๋ฐ์ ์์ฑ์๊ฐ ์์)  

**~~์ฌ์ค ๋ฑ ๋ณด์๋ง์ ์์๋ดค๋ค. ํ์ง๋ง ์ง์์ output์ ํด์ผ ์ค๋ ๊ธฐ์ต์ ๋จ๋๋ฒ.. ๊ทธ๋์ ์ ๋ฆฌํ๋ฉฐ ํฌ์คํํ๋ค.~~**

## ์์ฑ์๊ฐ ์กด์ฌํ๋๋ฐ, ์?

๋์น์ฑ์ ๋ถ๋ค์ ์์๊ฒ ์ง๋ง, spring์ ๊ธฐ๋ณธ์ ์ผ๋ก ์ง๋ ฌํ/์ญ์ง๋ ฌํ์ ์ญ์จ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๊ธฐ๋ณธ์ผ๋ก ์ฌ์ฉํ๊ณ  ์๊ณ  ๊ทธ ์ญ์จ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ ๊ธฐ๋ณธ์์ฑ์๊ฐ ์์ผ๋ฉด ๋์ํ์ง ์์ต๋๋ค.  

์, ์๋ฌ ๋ฉ์ธ์ง๋ฅผ ์ถ๋ ฅํ ํด๋์ค๋ฅผ ์ถ์ ํด๋ด์๋ค.  

```java
protected Object deserializeFromObjectUsingNonDefault(JsonParser p,
            DeserializationContext ctxt) throws IOException
{
    final JsonDeserializer<Object> delegateDeser = _delegateDeserializer();
    if (delegateDeser != null) {
        final Object bean = _valueInstantiator.createUsingDelegate(ctxt,
                delegateDeser.deserialize(p, ctxt));
        if (_injectables != null) {
            injectValues(ctxt, bean);
        }
        return bean;
    }
    if (_propertyBasedCreator != null) {
        return _deserializeUsingPropertyBased(p, ctxt);
    }
    // 25-Jan-2017, tatu: We do not actually support use of Creators for non-static
    //   inner classes -- with one and only one exception; that of default constructor!
    //   -- so let's indicate it
    Class<?> raw = _beanType.getRawClass();
    if (ClassUtil.isNonStaticInnerClass(raw)) {
        return ctxt.handleMissingInstantiator(raw, null, p, 
"non-static inner classes like this can only by instantiated using default, no-argument constructor");
    }
    return ctxt.handleMissingInstantiator(raw, getValueInstantiator(), p, 
"cannot deserialize from Object value (no delegate- or property-based Creator)");
}
```

์๋ฌ๋ก๊ทธ๋ฅผ ์ ๋ฌํ๋ ๋ฉ์๋ ์ด๋ฆ์ด `deserializeFromObjectUsingNonDefault` ์๋์ด๋ค.  
์ด๋ฆ๋ถํฐ ์ฌ์์น์๋ค.  

์ข๋ ์ฐพ์๋ณด์.

```java
/*
/**********************************************************
/* Public API implementation; instantiation from JSON Object
/**********************************************************
 */

@Override
public Object createUsingDefault(DeserializationContext ctxt) throws IOException
{
    if (_defaultCreator == null) { // sanity-check; caller should check
        return super.createUsingDefault(ctxt);
    }
    try {
        return _defaultCreator.call();
    } catch (Exception e) { // 19-Apr-2017, tatu: Let's not catch Errors, just Exceptions
        return ctxt.handleInstantiationProblem(_valueClass, null, rewrapCtorProblem(ctxt, e));
    }
}
```

๋ฉ์๋๋ฅผ ๊พธ์คํ ๋ฐ๋ผ๊ฐ๋ค๋ณด๋ ๋ฌธ์  ๋ฐ์ ๊ทผ์์ง๋ฅผ ์ฐพ์ ์ ์์๋ค.  

> if (_defaultCreator == null) { // sanity-check; caller should check

์ด ๋ถ๋ถ์ด ๋ฐ๋ก ๊ธฐ๋ณธ์์ฑ์๊ฐ null ์ธ์ง ํ์ธํ๊ณ  null์ด๋ฉด `super.createUsingDefault(ctxt);` ๋ฉ์๋๊ฐ ํธ์ถ๋  ๊ฒ์ด๊ณ  ์๋ฌ๋ก๊ทธ๊ฐ ์ถ๋ ฅ๋ ๊ณณ์ผ๋ก ๋ฐ์ธ๋ฉ๋์ด ์ค์  ์๋ฌ๋ก๊ทธ๊ฐ ์ถ๋ ฅ๋  ๊ฒ์๋๋ค.  

๋ฐ๋๋ก ๊ธฐ๋ณธ์์ฑ์๊ฐ ์๋ค๋ฉด ํธ์ถ๋๋ `_defaultCreator.call();` ๋ฅผ ๋ค์ด๊ฐ๋ณด๋ฉด ๊ธฐ๋ณธ์์ฑ์๋ฅผ ์ฌ์ฉํ๋์ง ์ดํด๋ณด๋ฉด ํ์คํ ์ ์ ์์ ๊ฒ ๊ฐ๋ค์.  

```java
@Override
public final Object call() throws Exception {
    // 31-Mar-2021, tatu: Note! This is faster than calling without arguments
    //   because JDK in its wisdom would otherwise allocate `new Object[0]` to pass
    return _constructor.newInstance((Object[]) null);
}
```

## ๊ทธ๋ ๋ค๋ฉด ์๋ฌ๋ก๊ทธ์ ์๋ no delegate- or property-based ๋ ๋ญ์ง?

์ฐ๋ฆฌ๊ฐ ์ง๋์ณ์จ ์ฝ๋์ ๋ต์ด ์กด์ฌํฉ๋๋ค.  
์์ ์ฝ๋ ์ค ์๋ฌ๋ฉ์ธ์ง ์ถ๋ ฅ๋ ๋ฉ์๋๋ฅผ ํ์ธํด๋ณผ๊น์?  

```java
protected Object deserializeFromObjectUsingNonDefault(JsonParser p,
            DeserializationContext ctxt) throws IOException
{
    final JsonDeserializer<Object> delegateDeser = _delegateDeserializer();
    if (delegateDeser != null) {
        final Object bean = _valueInstantiator.createUsingDelegate(ctxt,
                delegateDeser.deserialize(p, ctxt));
        if (_injectables != null) {
            injectValues(ctxt, bean);
        }
        return bean;
    }
    if (_propertyBasedCreator != null) {
        return _deserializeUsingPropertyBased(p, ctxt);
    }
    // 25-Jan-2017, tatu: We do not actually support use of Creators for non-static
    //   inner classes -- with one and only one exception; that of default constructor!
    //   -- so let's indicate it
    Class<?> raw = _beanType.getRawClass();
    if (ClassUtil.isNonStaticInnerClass(raw)) {
        return ctxt.handleMissingInstantiator(raw, null, p, 
"non-static inner classes like this can only by instantiated using default, no-argument constructor");
    }
    return ctxt.handleMissingInstantiator(raw, getValueInstantiator(), p, 
"cannot deserialize from Object value (no delegate- or property-based Creator)");
}
```

`deserializeFromObjectUsingNonDefault` ๋ฉ์๋๋ช์ ์ดํด๋ณด๋ฉด ๊ธฐ๋ณธ์์ฑ์ ์์ด ์ญ์ง๋ ฌํํ๋ ๋ฉ์๋์๋๋ค.  

์ด ๋ฉ์๋๊ฐ ํ๋ค๋ฉด ๋ฐ๋ก ์๋ฌ๋ฅผ ์ฐ์ด์ผํ๋๋ฐ ์ธ๋ถ ๋ก์ง์ ๋ณด๋ฉด  

```java
final JsonDeserializer<Object> delegateDeser = _delegateDeserializer();
if (delegateDeser != null) {
    final Object bean = _valueInstantiator.createUsingDelegate(ctxt,
            delegateDeser.deserialize(p, ctxt));
    if (_injectables != null) {
        injectValues(ctxt, bean);
    }
    return bean;
}
```

`delegateDeser != null` delegate๊ฐ ์กด์ฌํ๋ค๋ฉด `createUsingDelegate` ๋ฉ์๋๋ฅผ ํตํด ๊ฐ์ฒด๋ฅผ ์์ฑํ๋ ๋ก์ง์ด ๋ค์ด๊ฐ ์์ต๋๋ค.  
์ง๊ธ ์ ์ ์ํฉ์ `delegateDeser == null` ์ด๊ธฐ ๋๋ฌธ์ ๋ง์ง๋ง ๊ตฌ๋ฌธ์ ์๋ฌ๋ก๊ทธ๊ฐ ์ถ๋ ฅ๋ ๊ฒ์ ๋ณผ ์ ์์ต๋๋ค.  

๋ํ ๊ทธ ๋ฐ์ ๋ก์ง์ธ  

```java
if (_propertyBasedCreator != null) {
    return _deserializeUsingPropertyBased(p, ctxt);
}
```

`_propertyBasedCreator` ๋ ์๋๊ธฐ ๋๋ฌธ์ ์ ๋ฐ ์๋ฌ๊ฐ ์ฐํ๊ฒ์๋๋ค.  

## ์ ๋ฆฌ

๊ฒฐ๊ตญ, ์ ์ ๋ฌธ์ ๋ Controller์์ API ์์ฒญ ์คํ์ ์ ๋ฌํ๋ DTO์ ๊ฐ์ฒด๋ก์จ ๊ธฐ๋ณธ์์ฑ์๊ฐ ์์๊ธฐ์ ๋ํ๋ ๋ฌธ์ ์์ต๋๋ค.  
๊ฐ๋จํ ๊ธฐ๋ณธ์์ฑ์๋ฅผ ์ถ๊ฐํด์ ์๋ฌ๋ฅผ ํด๊ฒฐํ  ์ ์์์ต๋๋ค.  
๋กฌ๋ณต์ @builder๋ ๊ธฐ๋ณธ์์ฑ์๋ฅผ ์์ฑํด์ฃผ์ง ์๊ธฐ์ ์ด๋ฐ ์ํฉ์ด ๋ฐ์ํ์์ต๋๋ค.. ๐ฑ  

์ฐ๋ฆฌ ๋ชจ๋ ํ๋๋ง ๊ธฐ์ตํฉ์๋ค.  
์ญ์จ์ object mapper๋ ๊ธฐ๋ณธ์์ฑ์๋ฅผ ํ์๋กํ๋ค! (๊ธฐ๋ณธ์์ฑ์๋ `private` ์ด์ด๋ ๋๋ค! **๋ฆฌํ๋ ์**)  
๋ฒ์ธ๋ก `@JsonProperty`, `@JsonAutoDetect` ๋ฑ์ ์ฌ์ฉํ๋ ํ๋กํผํฐ ๊ธฐ๋ฐ ๊ฐ์ฒด or ์์ฑ์๋ฅผ ์์ํ ๊ฒฝ์ฐ๋ผ๋ฉด ํ์ํ์ง ์๋ค!  

๋.

```toc
```
