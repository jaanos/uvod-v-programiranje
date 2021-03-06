Rekurzija
=========

Rekurzivni klici
----------------

Videli smo, da lahko iz funkcij kličemo tudi druge funkcije. Na primer, v
funkciji ``povrsina_tetraedra`` smo poklicali funkcijo ``ploscina_trikotnika``,
v tej pa vgrajeno funkcijo ``math.sqrt``. V resnici pa lahko v funkciji
pokličemo tudi funkcijo samo. Takemu klicu pravimo **rekurzivni klic**.
Poglejmo, kako bi izračunali :math:`n! = n \cdot (n - 1) \dots 3 \cdot 2 \cdot
1`. Kot vidimo velja :math:`n! = n \cdot (n - 1)!`, zato bomo :math:`n!`
izračunali tako, da bomo :math:`n` pomnožili z :math:`(n - 1)!`. Toda od kod
bomo dobili tega? Preprosto, :math:`n - 1` bomo pomnožili z :math:`(n - 2)!`. Od
kod pa tega? Ja iz :math:`(n - 3)!`. In tako naprej vse do :math:`2!`, ki ga
bomo dobili iz :math:`1!`, tega pa iz :math:`0!`, ki je po definiciji enak
:math:`1`.

Torej lahko funkcijo, ki računa fakulteto, napišemo tako, da najprej pogleda
svoj argument ``n``. Če je enak 1, vrne 1, sicer pa izračunamo tako, da ``n``
pomnožimo z rezultatom klica ``fakulteta(n - 1)``:

.. testcode::

    def fakulteta(n):
        '''Vrne fakulteto naravnega števila n.'''
        if n == 0:
            return 1
        else:
            return n * fakulteta(n - 1)

ali s pogojnim izrazom kot:

.. testcode::

    def fakulteta(n):
        '''Vrne fakulteto naravnega števila n.'''
        return 1 if n == 0 else n * fakulteta(n - 1)

.. doctest::

    >>> fakulteta(1)
    1
    >>> fakulteta(5)
    120
    >>> fakulteta(10)
    3628800

Še en primer rekurzivne definicije so Fibonaccijeva števila. Velja :math:`F_0 = 0`,
:math:`F_1 = 1`, za vse :math:`n \ge 2` pa velja in :math:`F_{n} = F_{n - 1} + F_{n - 2}`.
Funkcijo tedaj napišemo podobno na podoben način kot zgornjo: če
je ``n`` enak 0, vrnemo 0, sicer pogledamo, ali je enak 1. V tem primeru vrnemo
1. Če pa tudi 1 ni enak, mora biti večji ali enak 2, zato se pokličemo
rekurzivno.

.. testcode::

    def fibonacci(n):
        '''Vrne n-to Fibonaccijevo število.'''
        if n == 0:
            return 0
        elif n == 1:
            return 1
        else:
            return fibonacci(n - 1) + fibonacci(n - 2)

.. doctest::

    >>> fibonacci(3)
    2
    >>> fibonacci(4)
    3
    >>> fibonacci(5)
    5
    >>> fibonacci(20)
    6765

Kaj se zgodi, če poskušate izračunati ``fibonacci(35)``? Po nekaj časa res dobite
pravilen odgovor 9227465, vendar to kaže, da nekaj ni v redu. Kmalu bomo videli
razloge, zakaj ta funkcija ni dobro napisana. Bolje bi bilo, če bi jo zastavili
malo drugače. Poleg Fibonaccijevega zaporedja, ki se začne s številoma 0 in 1,
obstajajo tudi splošno Fibonaccijevo zaporedje, ki se začne s poljubnima členoma
:math:`a` in :math:`b`:

.. math::

    a, b, a + b, b + (a + b) = a + 2 b, (a + b) + (a + 2 b) = 2 a + 3 b, \ldots

Vidimo, da je :math:`n`. člen tega zaporedja ravno :math:`n - 1`. člen zaporedja,
ki se začne s členoma :math:`b` in :math:`a + b`. Tedaj lahko definiramo:

.. testcode::

    def splosni_fibonacci(n, a, b):
        '''Vrne n-ti člen Fibonaccijevega zaporedja, ki se začne z a in b.'''
        if n == 0:
            return a
        elif n == 1:
            return b
        else:
            return splosni_fibonacci(n - 1, b, a + b)

Kot lahko sami preizkusimo, ta funkcija deluje veliko hitreje od prejšnje:

.. doctest::

    >>> splosni_fibonacci(35, 0, 1)
    9227465
    >>> splosni_fibonacci(25, 1, -1)
    -28657
    >>> splosni_fibonacci(25, 0, 2)
    150050
    >>> splosni_fibonacci(500, 0, 1)
    139423224561697880139724382870407283950070256587697307264108962948325571622863290691557658876222521294125

Pomembno ni torej samo to, da naš program pravilno izračuna iskani rezultat,
temveč tudi to, kako učinkovito ga izračuna.

Neobvezni argumenti
-------------------

Včasih imamo za nekatere argumente funkcij v mislih že prav določeno vrednost.
Na primer, za izračun logaritma potrebujemo dve števili: osnovo in argument
(tudi logaritmand). Toda velikokrat za osnovo vzamemo :math:`10`, zato namesto
:math:`\log_{10} x` pišemo kar :math:`\log x`. Tudi pri Pythonu je podobno. Če
se nam ob klicu funkcije ne ljubi navajati vrednosti vseh argumentov, lahko za
nekatere od njih v prvi vrstici definicije navedemo privzeto vrednost. Na primer, pri funkciji
``splosni_fibonacci`` želimo, da imata ``a`` in ``b`` privzeti vrednosti 0 in 1:

.. testcode::

    def splosni_fibonacci(n, a=0, b=1):
        '''Vrne n-ti člen Fibonaccijevega zaporedja, ki se začne z a in b.'''
        if n == 0:
            return a
        elif n == 1:
            return b
        else:
            return splosni_fibonacci(n - 1, b, a + b)

Tedaj se bo vedno uporabila privzeta vrednost za tiste argumente, ki jih ne
podamo izrecno.

    >>> splosni_fibonacci(35)
    9227465
    >>> splosni_fibonacci(500)
    139423224561697880139724382870407283950070256587697307264108962948325571622863290691557658876222521294125
    >>> splosni_fibonacci(25, b=2)
    150050
    >>> splosni_fibonacci(25, a=1, b=-1)
    -28657

Klic deluje tudi, če neobveznih argumentov ne poimenujemo, vendar lahko to vodi
do zmede, zato se takih klicev izogibamo.

.. doctest::

    >>> splosni_fibonacci(25, 1, -1)
    -28657


Stavek ``assert``
-----------------

Tudi funkcija ``splosni_fibonacci`` še ni popolna. Kaj se zgodi, če pokličemo
``splosni_fibonacci(-2)``? Ker -2 ni enako ne 0 ne 1, bomo izvedli tretjo
vejo pogojnega stavka in izračunali ``splosni_fibonacci(-3, ...)``, iz tega
pa podobno ``splosni_fibonacci(-4, ...)`` in tako naprej, vse do trenutka, ko
se bo Python pritožil:

.. doctest::

    >>> splosni_fibonacci(-2)
    Traceback (most recent call last):
      ...
      File "...", line 8, in splosni_fibonacci
      File "...", line 8, in splosni_fibonacci
      File "...", line 8, in splosni_fibonacci
      File "...", line 8, in splosni_fibonacci
      File "...", line 8, in splosni_fibonacci
      File "...", line 8, in splosni_fibonacci
      File "...", line 3, in splosni_fibonacci
    RecursionError: maximum recursion depth exceeded in comparison

Pravi nam, da je naša rekurzija šla pregloboko. O tem bomo še bolj natančno
govorili, zaenkrat pa naj nam tako opozorilo pove, da smo program napisali tako,
da se ne bo ustavil. Da podobne situacije preprečimo, lahko uporabimo stavek
``assert``, v katerem napišemo pogoj, ki mu mora program zadoščati. Če mu ne,
Python javi napako.

.. testcode::

    def splosni_fibonacci(n, a=0, b=1):
        '''Vrne n-ti člen Fibonaccijevega zaporedja, ki se začne z a in b.'''
        assert n >= 0
        if n == 0:
            return a
        elif n == 1:
            return b
        else:
            return splosni_fibonacci(n - 1, b, a + b)

.. doctest::

    >>> splosni_fibonacci(-2)
    Traceback (most recent call last):
      ...
    AssertionError

Še vedno dobimo napako, vendar je ta bolj obvladljiva, pa še takoj se pojavi.
Stavke ``assert`` uporabljamo, kadar v nadaljevanju programa pričakujemo, da
je nekim pogojem zadoščeno. Namesto ``assert pogoj`` bi seveda lahko pisali tudi
nekaj v stilu:

.. code::

    if not pogoj:
        ustavi_program
        javi_napako

ampak ker je to pogosto koristno, so v ta namen uvedli ``assert``.
