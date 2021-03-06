
Operacije na slovarjih
----------------------

.. code::

    >>> s = {'a': 6, 'b': 2, 'c': 3}
    >>> len(s)
    3
    >>> max(s)
    'c'
    >>> max(s.values())
    6
    >>> sum(s.values())
    11

.. code::

    >>> s = {'a': 6, 'b': 2, 'c': 3}
    >>> s
    {'a': 6, 'b': 2, 'c': 3}
    >>> s['a']
    6
    >>> s['b']
    2
    >>> s['d']
    Traceback (most recent call last):
      File "<pyshell#20>", line 1, in <module>
        s['d']
    KeyError: 'd'
    >>> s.get('a')
    6
    >>> s.get('d')
    >>> s.get('d', 0)
    0
    >>> 'b' in s
    True
    >>> 'd' in s
    False

.. testcode::

    def prestej_pojavitve(niz):
        pojavitve = {}
        for znak in niz:
            if znak in pojavitve:
                pojavitve[znak] += 1
            else:
                pojavitve[znak] = 1
        return pojavitve

    def prestej_pojavitve(niz):
        pojavitve = {}
        for znak in niz:
            pojavitve[znak] = pojavitve.get(znak, 0) + 1
        return pojavitve


.. code::

    >>> prestej_pojavitve('abrakadabra')



Spreminjanje slovarjev
----------------------

.. code::

    >>> s = {'a': 6, 'b': 2, 'c': 3}
    >>> s['b'] = 8
    >>> s
    {'b': 8, 'a': 6, 'c': 3}
    >>> s['d'] = 10
    >>> s
    {'b': 8, 'a': 6, 'c': 3, 'd': 10}
    >>> del s['b']
    >>> s
    {'d': 10, 'a': 6, 'c': 3}


Zanka ``for`` na slovarjih
--------------------------

.. code::

    >>> s = {'a': 6, 'b': 2, 'c': 3}
    >>> list(s.keys())
    ['b', 'a', 'c']
    >>> list(s.values())
    [2, 6, 3]
    >>> list(s.items())
    [('b', 2), ('a', 6), ('c', 3)]

.. testcode::

    def najvecja_vrednost(s):
        # Ideja je podobna kot pri seznamih: po vrsti gledamo vse pare ključev in
        # vrednosti v slovarju - vsakič, ko vidimo še večjo vrednost, si zapomnimo
        # njen ključ.

        # Ker slovarji niso urejeni po vrsti, ne moremo začeti s prvim elementom.
        # Lahko pa si pomagamo z metodo popitem(), ki iz slovarja odstrani naključen
        # ključ in njegovo vrednost.
        max_k, max_v = s.popitem()
        for k, v in s.items():
            if v > max_v:
                max_k, max_v = k, v
        return max_k, max_v


.. code::

    >>> {n: stevilo_deliteljev(n) for n in range(20, 29)}
    {20: 6, 21: 4, 22: 4, 23: 2, 24: 8, 25: 3, 26: 4, 27: 4, 28: 6}


Množice
-------


.. testcode::

    def je_prastevilo(n):
        if n <= 2:
            return n == 2
        d = 3
        while d * d <= n:
            if n % d == 0:
                return False
            d += 2
        return True

.. code::

    majhna_prastevila = {2, 3, 5, 7, ..., 997}

    def je_prastevilo(n):
        if n <= majhna_prastevila[-1]:
            return n in majhna_prastevila
        d = 3
        while d * d <= n:
            if n % d == 0:
                return False
            d += 2
        return True

.. code::

    >>> s = {6, 2, 3}
    >>> len(s)
    3
    >>> max(s)
    6
    >>> sum(s)
    11
    >>> sez = [1, 5, 2, 4, 8, 1, 5, 3, 2, 1]
    >>> sum(sez)
    32
    >>> mn = set(sez)
    >>> sum(mn)
    23


.. code::

    >>> a = {1, 2, 3, 4}
    >>> a.add(5)
    >>> a
    {1, 2, 3, 4, 5}
    >>> a.remove(3)
    >>> a
    {1, 2, 4, 5}


.. code::

    >>> {1, 2, 3, 4} | {3, 4, 5, 6}
    {1, 2, 3, 4, 5, 6}
    >>> {1, 2, 3, 4} & {3, 4, 5, 6}
    {3, 4}
    >>> {1, 2, 3, 4} - {3, 4, 5, 6}
    {1, 2}
    >>> 1 in {1, 2, 3, 4}
    True
    >>> 2 in {3, 4, 5, 6}
    False
    >>> list({1, 2, 3})


.. testcode::

    def direktna_slika(f, a):
        slika = set()
        for x in a:
            slika.add(f(x))
        return slika

    def direktna_slika(f, a):
        return {f(x) for x in a}

.. code::

    >>> direktna_slika(lambda x: x ** 2, {-5, -3, 1, 2})
    {1, 4, 9, 25}
    >>> {x ** 2 for x in range(1, 30) if x % 5 == 3}
    {64, 324, 9, 169, 784, 529}
    >>> {x for x in range(1, 100) if x % 5 == 3 and x % 3 != 0}
    {98, 68, 38, 8, 73, 43, 13, 83, 53, 23, 88, 58, 28}


.. testcode::

    {
        'Borut': {'Janez', 'Miro', 'Karl'},
        'Janez': {'Borut', 'Karl'},
        'Miro': {'Borut', 'Karl'},
        'Karl': {'Borut', 'Janez', 'Miro'},
        'Igor': set(),
    }

    def priporoci_prijatelje(omrezje, oseba):
        novi_prijatelji = set()
        for prijatelj in omrezje[oseba]:
            for prijatelj_prijatelja in omrezje[prijatelj]:
                novi_prijatelji.add(prijatelj_prijatelja)
        return novi_prijatelji - {oseba} - omrezje[oseba]
