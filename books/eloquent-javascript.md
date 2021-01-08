> NaN is supposed to denote the result of a nonsensical computation, and as such, it isn’t equal to the result of any other nonsensical computations.

> The || operator, for example, will return the value to its left when that can be converted to true and will return the value on its right otherwise. This has the expected effect when the values are Boolean and does something analogous for values of other types.

> The && operator works similarly but the other way around. When the value to its left is something that converts to false, it returns that value, and otherwise it returns the value on its right.

> Another important property of these two operators is that the part to their right is evaluated only when necessary.