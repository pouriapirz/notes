#notes

## GPerf

[gperf](https://www.gnu.org/software/gperf/) is a perfect hash function generator.

Desired mapping:
```
"first"  --> ItemA
"second" --> ItemB
"third"  --> ItemC
```

```
// ItemTag.h
enum class ItemTag
{
	ItemA,
	ItemB,
	ItemC
};
```

myInput.gperf
```
%{
#include "ItemTag.h"
%}
struct ParseToken
  {
  const char *Option;
  ItemTag eItemTag;
  };
%%
first, ItemTag::ItemA
second, ItemTag::ItemB
third, ItemTag::ItemC
```

gperf command
```
gperf\bin>  gperf -CGD -N ParseItemTag -K Option -L C++ -t myInput.gperf > ItemTagPerfecthash.h
```

Add generated .h file to project. Sample usage below:
```
#include "stdafx.h"
#include <stdio.h>
#include "ItemTagPerfectHash.h"


int main()
{
	const char* pszKey1 = "first";
	const char* pszKey2 = "second";
	const char* pszKey3 = "other";

	const ParseToken* pTag1 = Perfect_Hash::ParseItemTag(pszKey1, strlen(pszKey1));
	if (!pTag1 || pTag1->eItemTag != ItemTag::ItemA) { printf("Unknown tag '%s'\n", pszKey1); }

	const ParseToken* pTag2 = Perfect_Hash::ParseItemTag(pszKey2, strlen(pszKey2));
	if (!pTag2 || pTag2->eItemTag != ItemTag::ItemB) { printf("Unknown tag '%s'\n", pszKey2); }

	const ParseToken* pTag3 = Perfect_Hash::ParseItemTag(pszKey3, strlen(pszKey3));
	if (pTag3) { printf("Invalid tag '%s'\n", pszKey3); }

    return 0;
}
```

