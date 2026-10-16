# mach.lang.driver.hygiene

the compiler's own source held to its rules, read from the checkout: each module is
imported under one name and each name imports one module, no pub declaration is named
only in its own file unless its doc comment says what reaches it, driver imports nothing
of build, a file directly under src/lang is a foundation or the facade of a shadow
module, and the layers the imports form: the front and middle end reach no backend, the
target tree and the pipeline meet only in the contract, and no test import climbs above
its module's layer

