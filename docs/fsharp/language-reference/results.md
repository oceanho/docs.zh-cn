---
title: "结果 （F #）"
description: "了解如何使用 F # Result 类型可帮助你编写容错的代码。"
keywords: "visual f#, f#, 函数编程"
author: cartermp
ms.author: phcart
ms.date: 04/24/2017
ms.topic: language-reference
ms.prod: .net
ms.technology: devlang-fsharp
ms.devlang: fsharp
ms.assetid: a15b5cf1-9055-4481-918c-4c8a051b5829
ms.openlocfilehash: 26478ccfbf88d43c0b194e77d9aacc313515283f
ms.sourcegitcommit: 8d14e8c1b15009330c9880f8523686158924e1a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2017
---
# <a name="results"></a>结果

F # 4.1 开始，没有`Result<'T,'TFailure>`可用于编写可由容错错误代码的类型。

## <a name="syntax"></a>语法

```fsharp
// The definition of Result in FSharp.Core
[<StructuralEquality; StructuralComparison>]
[<CompiledName("FSharpResult`2")>]
[<Struct>]
type Result<'T,'TError> = 
    | Ok of ResultValue:'T 
    | Error of ErrorValue:'TError
```

## <a name="remarks"></a>备注

请注意，结果类型是[结构可区分的联合](discriminated-unions.md#struct-discriminated-unions)，即在 F # 4.1 中引入的另一个功能。  结构上相等语义在此处适用。

`Result`类型通常用于一元错误处理，这通常称为[铁路面向编程](https://swlaschin.gitbooks.io/fsharpforfunandprofit/content/posts/recipe-part2.html)F # 社区内。  下面的简单示例演示这种方法。

```fsharp
// Define a simple type which has fields that can be validated
type Request = 
    { Name: string
      Email: string }

// Define some logic for what defines a valid name.
//
// Generates a Result which is an Ok if the name validates;
// otherwise, it generates a Result which is an Error.
let validateName req =
    match req.Name with
    | null -> Error "No name found."
    | String.Empty -> Error "Name is empty."
    | "bananas" -> Error "Bananas is not a name."
    | _ -> Ok req

// Similarly, define some email validation logic.
let validateEmail req =
    match req.Email with
    | null -> Error "No email found."
    | String.Empty -> Error "Email is empty."
    | s when s.EndsWith("bananas.com") -> Error "No email from bananas.com is allowed."
    | _ -> OK req

let validateRequest reqResult =
    reqResult 
    |> Result.bind validateName
    |> Result.bind validateEmail

let test() = 
    // Now, create a Request and pattern match on the result.
    let req1 = { Name = "Phillip"; Email = "phillip@contoso.biz" }
    let res1 = validateRequest (OK req1)
    match res1 with
    | Ok req -> printfn "My request was valid! Name: %s Email %s" req1.Name req1.Email
    | Error e -> printfn "Error: %s" e
    // Prints " "My request was valid!  Name: Phillip Email: phillip@consoto.biz"

    let req2 = { Name = "Phillip"; Email = "phillip@bananas.com" }
    let res2 = validateRequest (OK req2)
    match res2 with
    | Ok req -> printfn "My request was valid! Name: %s Email %s" req1.Name req1.Email
    | Error e -> printfn "Error: %s" e
    // Prints: "Error: No email from bananas.com is allowed."

test()
```

如你所见，它是很容易地链接在一起验证的各种函数，如果强制其全部返回`Result`。  这样，分解如下分为小的部分是根据你的需要是可组合的功能。  此模式还具有增加的价值在于*强制实施*使用[模式匹配](pattern-matching.md)在一轮验证结束时，后者又在强制实施更高的程序正确性。

## <a name="see-also"></a>另请参阅

[可区分联合](discriminated-unions.md)

[模式匹配](pattern-matching.md)
