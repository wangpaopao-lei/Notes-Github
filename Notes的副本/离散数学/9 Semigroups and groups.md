### 9.0
Concept:
1. A collection of subjects with operations defined on the and the accompanying properties from a **mathematical structure** or **system**
> 它是对于各种数学对象用集合和关系的语言给出的统一形式，由若干集合、定义在集合上或集合间的关系、以及一组作为条件的公理组成。

> Example1:The collection of sets with the operations of union,intersection,and complement and their accompanying properties is a (discrete) mathematical structure.We denote this structure by [sets,$\cup,\cap$].

> 序结构、代数结构和拓扑结构是最主要的三大类结构。

2. Closure:*封闭* A structure is **closed with respect to an operation** if that operation always produces another member of the collection of objects. 
3. Binary operation:*二元运算* An operation that combines two objects is a binary operation.(unary operation一元运算)
4. Commutative:*交换律*。
5. Associative:*结合律*
6. Distributive Property:*分配律*
7. Identity for an operation:*单位元* A distinguished object e with the property that $x\spades e=e\spades x=x$.
8. Inverse:*逆元* If a binary operation $\spades$ has an identity e ,we say y is a $\spades$-inverse of x if $x\spades y=y\spades x=e$.

### 9.1 Binary operation二元运算
Concept:A binary operation on a set A is everywhere defined function $f:A\times A\rightarrow A$
> 笛卡尔积$A\times A$的子集定义了元素之间的关系(relation)，二元运算从某种程度上定义了关系的一种表达，因此它反映了元素之间的某种规则。
> 当我们在集合上定义二元运算，就是试图定义一些元素之间的一些规则。

Properties:
1. Since a binary operation is a function. **Only one element** of A is assigned to each ordered pair. 
2. **A is closed** under the operation * ,if a and b are elements in A, $a*b\in A$.
3. **Idempotent**：(等幂律)$a*a=a$.

### 9.2 Semigroups半群
Concept:
1. A **semigroup** is a nonempty set S together with an associative binary operation* defined on S.
-Denote the semigroup by (S,*) or when it is clear what operation * is, simply by S.  
2. *单位元、幺元* An element e in a semigroup(S,*) is called an **identity element** if $e*a=a*e=a$ for all $a\in S$.
3. *独异点、含幺半群* A **monoid** is a  semigroup(S,*) that has an identity.
4. Let (S,* ) be a semigroup and T be a subset of S. If T is closed under the operation*, then $(T,*)$is called a **subsemigroup** of $(S,*)$.  
5. Let $(S,*)$ be a monoid with identity e, and T be a nonempty subset of S. If T is closed under the operation* and **$e\in T$**, then $(T,*)$ is called a **submonoid** of $(S,*)$.
6. *同构映射* Let $(S,*)$ and $(T,*')$ be two semigroups. A function $f:S\to T$ is called an isomorphism from $(S,*)$ to $(T,*')$ if it is a  one-to-one correspondence from S to T, and $f(a*b)=f(a)*'f(b)$ for all a and b in S.
> 通俗记忆，先运算再映射和先映射再运算是等价的

>If f is an isomorphism from $(S,*)$ to $(T,*')$, then, since f is a one-to-one correspondence, $f^{-1}$ exists and is a one-to one correspondence from T to S.
 * To prove Isomorphic semigroups
    step1. Define a function $f:S\to T$ with dom(f)=S
    step2. Show that f is one-to-one
    step3. Show that f is onto
    step4. Show that $f(a*b)=f(a)*'f(b)$
> Let $(S,*)$ and $(T,*')$ be monoids with identities e and e' respectively, $f:S\to T$ be an isomorphic. Then $f(e)=e'$.

7. *同态* Let $(S,*)$ and $(T,*')$ be two semigroups. An everywhere-defined function $f:S\to T$ is called a **homomorphism** from $(S,*)$ to $(T,*')$, if $f(a*b)=f(a)*'f(b)$ for all a and b in S. (By dropping the condition of one-to-one and onto)
8. *同态像* If f is also onto, we say that T is a **homomorphic image** of S.
> 同构与同态之间的差异是同构必须是单射和满射，同构与同态两者都有积的像是像的积。

> Let $(S,*)$ and $(T,*')$ be monoids with identities e and e' respectively, $f:S\to T$ be an homomorphic. Then $f(e)=e'$.

**同构**是指具有相同的代数结构，代数结构由一个或多个集合、若干运算及一些运算规则所唯一确定。代数结构相同的含义是指：除了表示集合元素的符号有可能不同以外，**对应集合的元素个数相同，集合上的运算一致，运算规则也完全一样**。如果两个代数结构相同，则他们之间必然存在至少一个同构映射。

**同构映射要满足的条件**：
1. 一一对应
2. 保持代数结构的所有运算以及一些特殊元素如单位元、零元素等。

研究代数结构的主要目的是对其进行分类。同构的代数结构可以不加区分，把它们看成一样的。因此，代数结构的分类就是找出该代数结构的所有同构类。

同态放弃了onto和one-to-one的要求，所以两个代数结构不同构，这个不同主要表现在元素的个数，但是他们之间保持相同的运算。同态比同构更一般、广泛。

### 9.3 Products and Quotients of Semigroups积半群和商半群
Theorem:
1. If $(S,*)$ and $(T,*')$ are semigroups, then $(S\times T,*'')$ is a semigroup, where $*''$ is defined by $(S_1,t_1)*''(s_2,t_2)=（s_1*s_2,t_1*'t_2)$ 
Concept:
1. *同余关系*：An equivalence relation R on the semigroup $(S,*)$ is called a **congruence relation** if aRa' and bRb' imply (a*b)R(a'*b')..
> 同余关系是代数结构中的一种等价关系，即可以用等价类中的代表元素运算得到运算后的等价类。

### 9.4 Groups群
### 9.5 Products and Quotients of groups积群和商群

