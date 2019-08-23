---
title: Операторы приведения и тестирования типов — справочник по C#
description: Сведения об операторах C#, которые можно использовать для проверки типа результата выражения и преобразования его в другой тип, если потребуется.
ms.date: 06/21/2019
author: pkulikov
f1_keywords:
- is_CSharpKeyword
- as_CSharpKeyword
- ()_CSharpKeyword
- typeof_CSharpKeyword
helpviewer_keywords:
- type-testing operators [C#]
- conversion operators [C#]
- type conversion [C#]
- is operator [C#]
- as operator [C#]
- cast operator [C#]
- cast expression [C#]
- () operator [C#]
- typeof operator [C#]
ms.openlocfilehash: 6966d0b7a4f8a96bddb17ce2017fd53fc07ae922
ms.sourcegitcommit: 29a9b29d8b7d07b9c59d46628da754a8bff57fa4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2019
ms.locfileid: "69572318"
---
# <a name="type-testing-and-cast-operators-c-reference"></a><span data-ttu-id="53396-103">Операторы приведения и тестирования типов (справочник по C#)</span><span class="sxs-lookup"><span data-stu-id="53396-103">Type-testing and cast operators (C# reference)</span></span>

<span data-ttu-id="53396-104">Чтобы выполнить проверку или преобразование типов, можно использовать следующие операторы:</span><span class="sxs-lookup"><span data-stu-id="53396-104">You can use the following operators to perform type checking or type conversion:</span></span>

- <span data-ttu-id="53396-105">[оператор is](#is-operator) проверяет, совместим ли тип среды выполнения для определенного выражения с указанным типом;</span><span class="sxs-lookup"><span data-stu-id="53396-105">[is operator](#is-operator): to check if the runtime type of an expression is compatible with a given type</span></span>
- <span data-ttu-id="53396-106">[оператор as](#as-operator) явным образом преобразует выражение в указанный тип, если тип среды выполнения совместим с этим типом;</span><span class="sxs-lookup"><span data-stu-id="53396-106">[as operator](#as-operator): to explicitly convert an expression to a given type if its runtime type is compatible with that type</span></span>
- <span data-ttu-id="53396-107">[оператор приведения ()](#cast-operator-) выполняет явное преобразование;</span><span class="sxs-lookup"><span data-stu-id="53396-107">[cast operator ()](#cast-operator-): to perform an explicit conversion</span></span>
- <span data-ttu-id="53396-108">[оператор typeof](#typeof-operator) получает экземпляр <xref:System.Type?displayProperty=nameWithType> указанного типа.</span><span class="sxs-lookup"><span data-stu-id="53396-108">[typeof operator](#typeof-operator): to obtain the <xref:System.Type?displayProperty=nameWithType> instance for a type</span></span>

## <a name="is-operator"></a><span data-ttu-id="53396-109">Оператор is</span><span class="sxs-lookup"><span data-stu-id="53396-109">is operator</span></span>

<span data-ttu-id="53396-110">Оператор `is` проверяет, совместим ли тип среды выполнения для результата определенного выражения с указанным типом.</span><span class="sxs-lookup"><span data-stu-id="53396-110">The `is` operator checks if the runtime type of an expression result is compatible with a given type.</span></span> <span data-ttu-id="53396-111">Начиная с C# версии 7.0, оператор `is` также проверяет соответствие результата выражения указанному шаблону.</span><span class="sxs-lookup"><span data-stu-id="53396-111">Starting with C# 7.0, the `is` operator also tests an expression result against a pattern.</span></span>

<span data-ttu-id="53396-112">Выражение с оператором проверки типа `is` имеет следующий вид:</span><span class="sxs-lookup"><span data-stu-id="53396-112">The expression with the type-testing `is` operator has the following form</span></span>

```csharp
E is T
```

<span data-ttu-id="53396-113">где `E` представляет выражение, возвращающее значение, а `T` содержит имя или параметр типа.</span><span class="sxs-lookup"><span data-stu-id="53396-113">where `E` is an expression that returns a value and `T` is the name of a type or a type parameter.</span></span> <span data-ttu-id="53396-114">`E` не может использоваться как анонимный метод или лямбда-выражение.</span><span class="sxs-lookup"><span data-stu-id="53396-114">`E` cannot be an anonymous method or a lambda expression.</span></span>

<span data-ttu-id="53396-115">Выражение `E is T` возвращает значение `true`, если результат выражения `E` отличен от NULL и может быть преобразован в тип `T` путем преобразования ссылки, упаковки-преобразования или распаковки-преобразования. В противном случае он возвращает `false`.</span><span class="sxs-lookup"><span data-stu-id="53396-115">The `E is T` expression returns `true` if the result of `E` is non-null and can be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion; otherwise, it returns `false`.</span></span> <span data-ttu-id="53396-116">Оператор `is` не учитывает заданные пользователем преобразования.</span><span class="sxs-lookup"><span data-stu-id="53396-116">The `is` operator doesn't consider user-defined conversions.</span></span>

<span data-ttu-id="53396-117">В следующем примере показано, что оператор `is` возвращает `true`, если тип среды выполнения для результата выражения является производным от указанного типа, то есть между этими типами существует преобразование ссылки:</span><span class="sxs-lookup"><span data-stu-id="53396-117">The following example demonstrates that the `is` operator returns `true` if the runtime type of an expression result derives from a given type, that is, there exists a reference conversion between types:</span></span>

[!code-csharp[is with reference conversion](~/samples/csharp/language-reference/operators/TypeTestingAndConversionOperators.cs#IsWithReferenceConversion)]

<span data-ttu-id="53396-118">В следующем примере показано, что оператор `is` учитывает упаковку-преобразование и распаковку-преобразование, но не учитывает числовые преобразования:</span><span class="sxs-lookup"><span data-stu-id="53396-118">The next example shows that the `is` operator takes into account boxing and unboxing conversions but doesn't consider numeric conversions:</span></span>

[!code-csharp-interactive[is with int](~/samples/csharp/language-reference/operators/TypeTestingAndConversionOperators.cs#IsWithInt)]

<span data-ttu-id="53396-119">Дополнительные сведения о преобразованиях в C# см. в главе [о преобразованиях](~/_csharplang/spec/conversions.md) в [спецификации по языку C#](~/_csharplang/spec/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="53396-119">For information about C# conversions, see the [Conversions](~/_csharplang/spec/conversions.md) chapter of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

### <a name="type-testing-with-pattern-matching"></a><span data-ttu-id="53396-120">Тестирование типов с сопоставлением шаблонов</span><span class="sxs-lookup"><span data-stu-id="53396-120">Type testing with pattern matching</span></span>

<span data-ttu-id="53396-121">Начиная с C# версии 7.0, оператор `is` также проверяет соответствие результата выражения указанному шаблону.</span><span class="sxs-lookup"><span data-stu-id="53396-121">Starting with C# 7.0, the `is` operator also tests an expression result against a pattern.</span></span> <span data-ttu-id="53396-122">В частности он поддерживает шаблон типа в следующем формате:</span><span class="sxs-lookup"><span data-stu-id="53396-122">In particular, it supports the type pattern in the following form:</span></span>

```csharp
E is T v
```

<span data-ttu-id="53396-123">где `E` представляет выражение, возвращающее значение, `T` содержит имя или параметр типа, а `v` является новой локальной переменной с типом `T`.</span><span class="sxs-lookup"><span data-stu-id="53396-123">where `E` is an expression that returns a value, `T` is the name of a type or a type parameter, and `v` is a new local variable of type `T`.</span></span> <span data-ttu-id="53396-124">Если результат выражения `E` отличен от NULL и может быть преобразован в тип `T` путем преобразования ссылки, упаковки-преобразования или распаковки-преобразования, выражение `E is T v` возвращает `true` и сохраняет преобразованное значение результата `E` в переменной `v`.</span><span class="sxs-lookup"><span data-stu-id="53396-124">If the result of `E` is non-null and can be converted to `T` by a reference, boxing, or unboxing conversion, the `E is T v` expression returns `true` and the converted value of the result of `E` is assigned to variable `v`.</span></span>

<span data-ttu-id="53396-125">В следующем примере показано использование оператора `is` с шаблоном типа:</span><span class="sxs-lookup"><span data-stu-id="53396-125">The following example demonstrates the usage of the `is` operator with the type pattern:</span></span>

[!code-csharp-interactive[is with type pattern](~/samples/csharp/language-reference/operators/TypeTestingAndConversionOperators.cs#IsTypePattern)]

<span data-ttu-id="53396-126">Дополнительные сведения о шаблонах типов и других поддерживаемых шаблонах см. в разделе [Сопоставление шаблонов с is](../keywords/is.md#pattern-matching-with-is).</span><span class="sxs-lookup"><span data-stu-id="53396-126">For more information about the type pattern and other supported patterns, see [Pattern matching with is](../keywords/is.md#pattern-matching-with-is).</span></span>

## <a name="as-operator"></a><span data-ttu-id="53396-127">Оператор as</span><span class="sxs-lookup"><span data-stu-id="53396-127">as operator</span></span>

<span data-ttu-id="53396-128">Оператор `as` явным образом преобразует результат выражения в указанный ссылочный или поддерживающий значения NULL тип.</span><span class="sxs-lookup"><span data-stu-id="53396-128">The `as` operator explicitly converts the result of an expression to a given reference or nullable value type.</span></span> <span data-ttu-id="53396-129">Если такое преобразование невозможно, оператор `as` возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="53396-129">If the conversion is not possible, the `as` operator returns `null`.</span></span> <span data-ttu-id="53396-130">В отличие от [оператора приведения ()](#cast-operator-),оператор `as` никогда не создает исключения.</span><span class="sxs-lookup"><span data-stu-id="53396-130">Unlike the [cast operator ()](#cast-operator-), the `as` operator never throws an exception.</span></span>

<span data-ttu-id="53396-131">Выражение имеет такой формат:</span><span class="sxs-lookup"><span data-stu-id="53396-131">The expression of the form</span></span>

```csharp
E as T
```

<span data-ttu-id="53396-132">где `E` представляет выражение, возвращающее значение, а `T` содержит имя или параметр типа. Результат такого выражения аналогичен результату этого:</span><span class="sxs-lookup"><span data-stu-id="53396-132">where `E` is an expression that returns a value and `T` is the name of a type or a type parameter, produces the same result as</span></span>

```csharp
E is T ? (T)(E) : (T)null
```

<span data-ttu-id="53396-133">за исключением того, что `E` вычисляется только один раз.</span><span class="sxs-lookup"><span data-stu-id="53396-133">except that `E` is only evaluated once.</span></span>

<span data-ttu-id="53396-134">Оператор `as` рассматривает только преобразование ссылки, допускающие значение NULL преобразования, упаковку-преобразование и распаковку-преобразование.</span><span class="sxs-lookup"><span data-stu-id="53396-134">The `as` operator considers only reference, nullable, boxing, and unboxing conversions.</span></span> <span data-ttu-id="53396-135">Нельзя использовать оператор `as` для определенного пользователем преобразования.</span><span class="sxs-lookup"><span data-stu-id="53396-135">You cannot use the `as` operator to perform a user-defined conversion.</span></span> <span data-ttu-id="53396-136">Для этого воспользуйтесь [оператором приведения ()](#cast-operator-).</span><span class="sxs-lookup"><span data-stu-id="53396-136">To do that, use the [cast operator ()](#cast-operator-).</span></span>

<span data-ttu-id="53396-137">В следующем примере иллюстрируется использование оператора `as`.</span><span class="sxs-lookup"><span data-stu-id="53396-137">The following example demonstrates the usage of the `as` operator:</span></span>

[!code-csharp-interactive[as operator](~/samples/csharp/language-reference/operators/TypeTestingAndConversionOperators.cs#AsOperator)]

> [!NOTE]
> <span data-ttu-id="53396-138">Как показано в примере выше, нужно сравнить результат выражения `as` со значением `null`, чтобы проверить, успешно ли выполнено преобразование.</span><span class="sxs-lookup"><span data-stu-id="53396-138">As the preceding example shows, you need to compare the result of the `as` expression with `null` to check if the conversion is successful.</span></span> <span data-ttu-id="53396-139">Начиная с C# версии 7.0, вы можете использовать [оператор is](#type-testing-with-pattern-matching) как для проверки успешного выполнения преобразования, так и для сохранения результата в переменной, если преобразование выполнено успешно.</span><span class="sxs-lookup"><span data-stu-id="53396-139">Starting with C# 7.0, you can use the [is operator](#type-testing-with-pattern-matching) both to test if the conversion succeeds and, if it succeeds, assign its result to a new variable.</span></span>

## <a name="cast-operator-"></a><span data-ttu-id="53396-140">Оператор приведения ()</span><span class="sxs-lookup"><span data-stu-id="53396-140">Cast operator ()</span></span>

<span data-ttu-id="53396-141">Выражение приведения в формате `(T)E` выполняет явное преобразование значения выражения `E` в тип `T`.</span><span class="sxs-lookup"><span data-stu-id="53396-141">A cast expression of the form `(T)E` performs an explicit conversion of the result of expression `E` to type `T`.</span></span> <span data-ttu-id="53396-142">Если явного преобразования из типа `E` в тип `T` не существует, возникает ошибка времени компиляции.</span><span class="sxs-lookup"><span data-stu-id="53396-142">If no explicit conversion exists from the type of `E` to type `T`, a compile-time error occurs.</span></span> <span data-ttu-id="53396-143">Во время выполнения явное преобразование может завершиться сбоем, и выражение приведения может вызвать исключение.</span><span class="sxs-lookup"><span data-stu-id="53396-143">At run time, an explicit conversion might not succeed and a cast expression might throw an exception.</span></span>

<span data-ttu-id="53396-144">Приведенный ниже пример демонстрирует явное числовое преобразование и преобразование ссылки:</span><span class="sxs-lookup"><span data-stu-id="53396-144">The following example demonstrates explicit numeric and reference conversions:</span></span>

[!code-csharp-interactive[cast expression](~/samples/csharp/language-reference/operators/TypeTestingAndConversionOperators.cs#Cast)]

<span data-ttu-id="53396-145">Сведения о поддерживаемых явных преобразованиях см. в разделе [о явных преобразованиях](~/_csharplang/spec/conversions.md#explicit-conversions) в [спецификации по языку C#](~/_csharplang/spec/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="53396-145">For information about supported explicit conversions, see the [Explicit conversions](~/_csharplang/spec/conversions.md#explicit-conversions) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span> <span data-ttu-id="53396-146">Сведения о том, как определять пользовательские операторы явного или неявного преобразования, см. в разделе [Операторы пользовательского преобразования](user-defined-conversion-operators.md).</span><span class="sxs-lookup"><span data-stu-id="53396-146">For information about how to define a custom explicit or implicit type conversion, see [User-defined conversion operators](user-defined-conversion-operators.md).</span></span>

### <a name="other-usages-of-"></a><span data-ttu-id="53396-147">Другие данные об использовании ()</span><span class="sxs-lookup"><span data-stu-id="53396-147">Other usages of ()</span></span>

<span data-ttu-id="53396-148">Используйте скобки, чтобы [вызвать метод или делегат](member-access-operators.md#invocation-operator-).</span><span class="sxs-lookup"><span data-stu-id="53396-148">You also use parentheses to [call a method or invoke a delegate](member-access-operators.md#invocation-operator-).</span></span>

<span data-ttu-id="53396-149">Кроме того, с помощью круглых скобок можно указывать порядок выполнения операций в выражении.</span><span class="sxs-lookup"><span data-stu-id="53396-149">Other use of parentheses is to specify the order in which to evaluate operations in an expression.</span></span> <span data-ttu-id="53396-150">Дополнительные сведения см. в разделе [Добавление скобок](../../programming-guide/statements-expressions-operators/operators.md#adding-parentheses) руководства по использованию [операторов](../../programming-guide/statements-expressions-operators/operators.md).</span><span class="sxs-lookup"><span data-stu-id="53396-150">For more information, see the [Adding parentheses](../../programming-guide/statements-expressions-operators/operators.md#adding-parentheses) section of the [Operators](../../programming-guide/statements-expressions-operators/operators.md) article.</span></span> <span data-ttu-id="53396-151">Список операторов, упорядоченных по уровню приоритета, см. в статье [Операторы в C#](index.md).</span><span class="sxs-lookup"><span data-stu-id="53396-151">For the list of operators ordered by precedence level, see [C# operators](index.md).</span></span>

## <a name="typeof-operator"></a><span data-ttu-id="53396-152">Оператор typeof</span><span class="sxs-lookup"><span data-stu-id="53396-152">typeof operator</span></span>

<span data-ttu-id="53396-153">Оператор `typeof` получает экземпляр <xref:System.Type?displayProperty=nameWithType> для указанного типа.</span><span class="sxs-lookup"><span data-stu-id="53396-153">The `typeof` operator obtains the <xref:System.Type?displayProperty=nameWithType> instance for a type.</span></span> <span data-ttu-id="53396-154">Оператор `typeof` принимает в качестве аргумента имя типа или параметр типа, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="53396-154">The argument to the `typeof` operator must be the name of a type or a type parameter, as the following example shows:</span></span>

[!code-csharp-interactive[typeof operator](~/samples/csharp/language-reference/operators/TypeTestingAndConversionOperators.cs#TypeOf)]

<span data-ttu-id="53396-155">Кроме того, оператор `typeof` можно использовать с несвязанными универсальными типами.</span><span class="sxs-lookup"><span data-stu-id="53396-155">You also can use the `typeof` operator with unbound generic types.</span></span> <span data-ttu-id="53396-156">В имени несвязанного универсального типа должно содержаться правильное количество запятых, то есть на одну меньше, чем число параметров этого типа.</span><span class="sxs-lookup"><span data-stu-id="53396-156">The name of an unbound generic type must contain the appropriate number of commas, which is one less than the number of type parameters.</span></span> <span data-ttu-id="53396-157">В следующем примере показано использование оператора `typeof` с несвязанным универсальным типом:</span><span class="sxs-lookup"><span data-stu-id="53396-157">The following example shows the usage of the `typeof` operator with an unbound generic type:</span></span>

[!code-csharp-interactive[typeof unbound generic](~/samples/csharp/language-reference/operators/TypeTestingAndConversionOperators.cs#TypeOfUnboundGeneric)]

<span data-ttu-id="53396-158">Оператор `typeof` не может принимать выражения в качестве аргументов.</span><span class="sxs-lookup"><span data-stu-id="53396-158">An expression cannot be an argument of the `typeof` operator.</span></span> <span data-ttu-id="53396-159">Чтобы получить экземпляр <xref:System.Type?displayProperty=nameWithType> типа среды выполнения для результата выражения, используйте метод <xref:System.Object.GetType%2A?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="53396-159">To get the <xref:System.Type?displayProperty=nameWithType> instance for the runtime type of the expression result, use the <xref:System.Object.GetType%2A?displayProperty=nameWithType> method.</span></span>

### <a name="type-testing-with-the-typeof-operator"></a><span data-ttu-id="53396-160">Тестирование типов с оператором `typeof`</span><span class="sxs-lookup"><span data-stu-id="53396-160">Type testing with the `typeof` operator</span></span>

<span data-ttu-id="53396-161">Используйте оператор `typeof` для проверки, совместим ли тип среды выполнения для определенного выражения с указанным типом.</span><span class="sxs-lookup"><span data-stu-id="53396-161">Use the `typeof` operator to check if the runtime type of the expression result exactly matches a given type.</span></span> <span data-ttu-id="53396-162">В следующем примере показано различие между проверкой типов с помощью оператора `typeof` и [оператора is](#is-operator):</span><span class="sxs-lookup"><span data-stu-id="53396-162">The following example demonstrates the difference between type checking performed with the `typeof` operator and the [is operator](#is-operator):</span></span>

[!code-csharp[typeof vs is](~/samples/csharp/language-reference/operators/TypeTestingAndConversionOperators.cs#TypeCheckWithTypeOf)]

## <a name="operator-overloadability"></a><span data-ttu-id="53396-163">Возможность перегрузки оператора</span><span class="sxs-lookup"><span data-stu-id="53396-163">Operator overloadability</span></span>

<span data-ttu-id="53396-164">Операторы `is`, `as` и `typeof` не поддерживают перегрузку.</span><span class="sxs-lookup"><span data-stu-id="53396-164">The `is`, `as`, and `typeof` operators are not overloadable.</span></span>

<span data-ttu-id="53396-165">Определяемый пользователем тип нельзя использовать для перегрузки оператора `()`, но на его основе можно определить пользовательское преобразование типа и выполнить его с помощью выражения приведения.</span><span class="sxs-lookup"><span data-stu-id="53396-165">A user-defined type cannot overload the `()` operator, but can define custom type conversions that can be performed by a cast expression.</span></span> <span data-ttu-id="53396-166">Дополнительные сведения см. в разделе [Операторы пользовательского преобразования](user-defined-conversion-operators.md).</span><span class="sxs-lookup"><span data-stu-id="53396-166">For more information, see [User-defined conversion operators](user-defined-conversion-operators.md).</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="53396-167">Спецификация языка C#</span><span class="sxs-lookup"><span data-stu-id="53396-167">C# language specification</span></span>

<span data-ttu-id="53396-168">Дополнительные сведения см. в следующих разделах статьи [Спецификация языка C#](~/_csharplang/spec/introduction.md):</span><span class="sxs-lookup"><span data-stu-id="53396-168">For more information, see the following sections of the [C# language specification](~/_csharplang/spec/introduction.md):</span></span>

- [<span data-ttu-id="53396-169">Оператор is</span><span class="sxs-lookup"><span data-stu-id="53396-169">The is operator</span></span>](~/_csharplang/spec/expressions.md#the-is-operator)
- [<span data-ttu-id="53396-170">Оператор as</span><span class="sxs-lookup"><span data-stu-id="53396-170">The as operator</span></span>](~/_csharplang/spec/expressions.md#the-as-operator)
- [<span data-ttu-id="53396-171">Выражения приведения</span><span class="sxs-lookup"><span data-stu-id="53396-171">Cast expressions</span></span>](~/_csharplang/spec/expressions.md#cast-expressions)
- [<span data-ttu-id="53396-172">Оператор typeof</span><span class="sxs-lookup"><span data-stu-id="53396-172">The typeof operator</span></span>](~/_csharplang/spec/expressions.md#the-typeof-operator)

## <a name="see-also"></a><span data-ttu-id="53396-173">См. также</span><span class="sxs-lookup"><span data-stu-id="53396-173">See also</span></span>

- [<span data-ttu-id="53396-174">справочник по C#</span><span class="sxs-lookup"><span data-stu-id="53396-174">C# reference</span></span>](../index.md)
- [<span data-ttu-id="53396-175">Операторы в C#</span><span class="sxs-lookup"><span data-stu-id="53396-175">C# operators</span></span>](index.md)
- [<span data-ttu-id="53396-176">Практическое руководство. Безопасное приведение с помощью сопоставления шаблонов и операторы is и as</span><span class="sxs-lookup"><span data-stu-id="53396-176">How to: safely cast by using pattern matching and the is and as operators</span></span>](../../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md)