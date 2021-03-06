Nizi
====

V programih seveda ne delamo le s števili, temveč tudi z drugimi podatki, na
primer besedili. V ta namen Python podpira nize, ki so strnjena zaporedja
znakov.

Pisanje nizov
-------------

Nize običajno pišemo v enojnih narekovajih, na primer ``'to je primer niza'``
ali pa ``'Mama je rekla: "Dober dan!"'``. Nize lahko pišemo tudi z dvojnimi
narekovaji, ki jih ponavadi uporabimo takrat, kadar v nizu želimo uporabiti
enojni narekovaj: ``"U Lublan' pr' Šestic'"``. V tem primeru niza ne moremo
pisati med enojnimi narekovaji, saj bi Python po narekovaju za ``Lublan``
mislil, da je konec niza.

Včasih želimo uporabiti obe vrsti narekovajev. V tem primeru si pomagamo z
*ubežnimi znaki*. To so znaki, ki jih na običajni način ne moremo zapisati, zato
uporabimo poseben zapis, ki se začne z znakom ``\``. Tedaj lahko pišemo ``'Mama
je iznenada začela peti: "U Lublan\' pr\' Šestic\'…"'`` ali pa ``"Mama je
iznenada začela peti: \"U Lublan' pr' Šestic'…\""``. Ubežne znake brez težav
lahko pišemo tudi tedaj, kadar ni treba ``'Mama je rekla: \"Dober dan!\"'``. Z
ubežnimi znaki lahko zapišemo tudi znak za novo vrstico ``\n``, za tabulator
``\t`` in seveda tudi za poševnico ``\\``, saj je ne moremo pisati le kot ``\``,
ker bi Python to razumel kot začetek ubežnega znaka.

Nize lahko pišemo tudi med tri enojne (``'''``) ali tri dvojne (``"""``)
narekovaje. V tem primeru za en sam narekovaj ne potrebujemo ubežnega znaka.
Take nize lahko pišemo tudi čez več vrstic.


Operacije na nizih
------------------

Nize lahko stikamo z operacijo ``+`` in množimo s celimi števili:

.. doctest::

    >>> 'Dober' + 'dan'
    'Doberdan'
    >>> 10 * 'ha'
    'hahahahahahahahahaha'
    >>> 'tro' + 5 * 'lo'
    'trolololololo'

Dolžino niza dobimo s funkcijo ``len``:

.. doctest::

    >>> len('Uvod v programiranje')
    20
    >>> len('')
    0

Na nizih imamo na voljo tudi predikat ``in``, s katerim ugotovimo, ali se
nek niz pojavlja kot podniz v drugem nizu. Na voljo je tudi ``not in``, s katerim
bolj berljivo zapišemo ravno nasprotno stvar:

.. doctest::

    >>> 'gram' in 'Uvod v programiranje'
    True
    >>> 'liter' in 'Uvod v programiranje'
    False
    >>> not ('liter' in 'Uvod v programiranje')
    True
    >>> 'liter' not in 'Uvod v programiranje'
    True


.. testcode::

    def je_samoglasnik(crka):
        return crka in 'aeiouAEIOU'


Indeksiranje
------------

Do posameznega znaka v nizu pridemo z *indeksiranjem*. Z izrazom ``niz[i]``
dostopamo do ``i``-tega znaka v danem nizu:

.. doctest::

    >>> najboljsi_predmet = 'Uvod v programiranje'
    >>> najboljsi_predmet[0]
    'U'
    >>> najboljsi_predmet[7]
    'p'
    >>> najboljsi_predmet[-1]
    'e'
    >>> najboljsi_predmet[-5]
    'r'

Indeksi se začnejo šteti z 0. Če uporabimo negativna števila, lahko štejemo tudi
od zadaj, vendar tam začnemo šteti z -1 (saj je -0 = 0).

.. code::

    0   1   2   3   4   5   6   7   8   9
    M   a   t   e   m   a   t   i   k   a
   -10 -9  -8  -7  -6  -5  -4  -3  -2  -1

Rezine
------

Na podoben način lahko dostopamo tudi do podnizov. Če napišemo ``niz[i:j]`` bomo
dobili niz od vključno ``i``-tega do vključno ``j - 1``-tega znaka. Če kakšno od
meja izpustimo, bomo vzeli vse znake od začetka oziroma do konca. Pišemo lahko
tudi ``niz[i:j:k]``, s čimer vzamemo le vsak ``k``-ti znak:

.. doctest::

    >>> najboljsi_predmet[10:14]
    'gram'
    >>> najboljsi_predmet[7:]
    'programiranje'
    >>> najboljsi_predmet[5:15:2]
    'vporm'
    >>> najboljsi_predmet[7::3]
    'pgmae'
    >>> najboljsi_predmet[::-1]
    'ejnarimargorp v dovU'
