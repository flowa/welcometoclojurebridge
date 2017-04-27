# Kuinka kävelyttää kilpikonnia

Ohessa muutama ohje perusteista monimutkaisempiin konsepteihin koskien
kilpikonnien liikuttamista.

Anna mielikuvituksesi lentää.

#### 1. Aloitetaan

- valmistelut

Jos et ole kloonannut tätä repositoryä, aja seuraava komento terminaalissa:

```bash
git clone https://github.com/Flowa/welcometoclojurebridge
cd welcometoclojurebridge
```

- lataa walk.clj Nightcodessa

Avaa tiedosto
`welcomeclojurebridge/src/clojurebridge_turtle/walk.clj`.
Klikkaa `Run with REPL`.
Tämä voi kestää hetken, mutta jonkin ajan kuluttua näät kehotteen `user=> nil` 
REPL ikkunan pohjalla. Tämän jälkeen klikkaa `Reload File`. Näet pienen ikkunan,
jonka keskellä on kolmio. Tämä kolmio on kilpikonna.

Jahka turtles-applikaatio on käynnissä, voit evaluoida lausekkeita (koodia
joka on sulkeiden välissä) seuraavilla tavoilla:

1. kirjoita koodia editorissa, valitse/maalaa alue ja klikkaa `Reload Selection`.
2. kirjoita koodisi REPL-ikkunaan ja paina (enter).

Näillä tavoilla on se ero, että mikäli kirjoitat
koodisi editoriin, voit tallentaa sen. Paina `Save`
ylävalikosta mikäli haluat tehdä näin.
Suoraan REPL:n kirjoittaminen on kätevää, mutta 
tätä koodia ei voi tallentaa.

- walk.clj:n lataaminen lein repl.ssä

käynnistä repl, sitten aja `require` ja `ns`.

```clojure
user=> (require 'clojurebridge-turtle.walk)
nil
user=> (ns clojurebridge-turtle.walk)
nil
clojurebridge-turtle.walk=> (turtle-names)
[:trinity]
clojurebridge-turtle.walk=> (state)
{:trinity {:x 0, :y 0, :angle 90, :color [106 40 126]}}
```

- alkutila

![initial state](img/initial-state.png)


[TURTLE.md](TURTLE.md) tiedostosta löydät komentoja joita kilpikonna ymmärtää


- `undo`, `clean`, ja `home`

Mikäli kilpikonna on kävellyt odottamattoman pitkän tai lyhyen matkan, komennon
voi peruuttaa `undo` funktiolla.
Mikäli kilpikonnan haluaa palauttaa alku-asemaan, se onnistuu perättäisillä
`clean` ja `home` funktiokutsuilla.

- `state`

Kun kilpikonna menee reunojen yli, tai et muutoin löydä tiettyä 
kilpikonnaa, voit tarkistaa niiden tilan `state` funktion avulla.
Komento palauttaa absoluuttisia arvoja.

[huom]  `forward`/`backward` tai `right`/`left` komentojen arvot ovat
riippuvaisia nykyisen tilan arvoista. Eivät absoluuttisia arvoja.

- `doc`

Jos haluamme tietää mitä jokkin funktio tekee, voimme käyttää Clojuren
`doc` komentoa. Esimerkiksi `(doc forward)` näyttää forward-funktion 
dokumentaation.

#### 2. [helppo] Perus liikkeet - forward, backward, right ja left

- forward

```clojure
clojurebridge-turtle.walk=> (forward 40)
{:trinity {:length 40}}
```

![forward 40](img/forward40.png)


- right

```clojure
clojurebridge-turtle.walk=> (right 90)
{:trinity {:angle 90}}
```

![right 90](img/right90.png)

- yhdistelmä funktioista forward ja right

```clojure
clojurebridge-turtle.walk=> (forward 40)
{:trinity {:length 40}}
clojurebridge-turtle.walk=> (right 90)
{:trinity {:angle 90}}
clojurebridge-turtle.walk=> (forward 50)
{:trinity {:length 50}}
clojurebridge-turtle.walk=> (right 90)
{:trinity {:angle 90}}
clojurebridge-turtle.walk=> (forward 60)
{:trinity {:length 60}}
clojurebridge-turtle.walk=> (right 90)
{:trinity {:angle 90}}
clojurebridge-turtle.walk=> (forward 70)
{:trinity {:length 70}}
clojurebridge-turtle.walk=> (right 90)
{:trinity {:angle 90}}
clojurebridge-turtle.walk=> (forward 80)
{:trinity {:length 80}}
```

![combination](img/forwardsandrights.png)


- yhdistelmä erilaisista komennoista

```clojure
clojurebridge-turtle.walk=> (init)
:trinity
clojurebridge-turtle.walk=> (forward 50)
{:trinity {:length 50}}
clojurebridge-turtle.walk=> (right 45)
{:trinity {:angle 45}}
clojurebridge-turtle.walk=> (backward 100)
{:trinity {:length -100}}
clojurebridge-turtle.walk=> (left 45)
{:trinity {:angle -45}}
clojurebridge-turtle.walk=> (forward 50)
{:trinity {:length 50}}
clojurebridge-turtle.walk=> (state)
{:trinity {:x -70.71068094436272, :y 29.289320335914155, :angle 90, :color [106 40 126]}}
```

![movement sample](img/various-combination.png)



#### 3. [helppo] Useita kilpikonnia

- lisää kilpikonnia

```clojure
clojurebridge-turtle.walk=> (init)
:trinity
clojurebridge-turtle.walk=> (add-turtle :neo)
{:neo {:x 0, :y 0, :angle 90, :color [12 84 0]}}
clojurebridge-turtle.walk=> (add-turtle :oracle)
{:oracle {:x 0, :y 0, :angle 90, :color [189 63 0]}}
clojurebridge-turtle.walk=> (add-turtle :cypher)
{:cypher {:x 0, :y 0, :angle 90, :color [75 102 125]}}
clojurebridge-turtle.walk=> (turtle-names)
[:trinity :neo :oracle :cypher]
```

![four turtles](img/four-turtles.png)

- muuta kilpikonnien kulmia

```clojure
clojurebridge-turtle.walk=> (right :neo (* 1 45))
{:neo {:angle 45}}
clojurebridge-turtle.walk=> (right :oracle (* 2 45))
{:oracle {:angle 90}}
clojurebridge-turtle.walk=> (right :cypher (* 3 45))
{:cypher {:angle 135}}
```

![four directions](img/four-directions.png)

- kävelytä neljää kilpikonnaa eteenpäin

```clojure
clojurebridge-turtle.walk=> (forward :trinity 40)
{:trinity {:length 40}}
clojurebridge-turtle.walk=> (forward :neo 40)
{:neo {:length 40}}
clojurebridge-turtle.walk=> (forward :oracle 40)
{:oracle {:length 40}}
clojurebridge-turtle.walk=> (forward :cypher 40)
{:cypher {:length 40}}
```

![four moves](img/four-forwards.png)


#### 4. [helppo] lisää vielä yksi kilpikonna ja anna sille komentoja

- lisää kilpokonna nimeltä :morpheus ja anna sille väri

```clojure
clojurebridge-turtle.walk=> (add-turtle :morpheus [21, 137, 255])
{:morpheus {:x 0, :y 0, :angle 90, :color [21 137 255]}}
```

![fifth turtle](img/fifth-turtle.png)


- käännä ja liikuta :morpheusta eteenpäin

```clojure
clojurebridge-turtle.walk=> (left :morpheus 45)
{:morpheus {:angle -45}}
clojurebridge-turtle.walk=> (forward :morpheus 40)
{:morpheus {:length 40}}
clojurebridge-turtle.walk=> (turtle-names)
[:trinity :neo :oracle :cypher :morpheus]
```

![fifth's move](img/fifth-turtle-move.png)


- liikuta viittä kilpikonnaa eteenpäin 50

```clojure
clojurebridge-turtle.walk=> (forward :trinity 20)
{:trinity {:length 20}}
clojurebridge-turtle.walk=> (forward :neo 20)
{:neo {:length 20}}
clojurebridge-turtle.walk=> (forward :oracle 20)
{:oracle {:length 20}}
clojurebridge-turtle.walk=> (forward :cypher 20)
{:cypher {:length 20}}
clojurebridge-turtle.walk=> (forward :morpheus 20)
{:morpheus {:length 20}}
```

![forward 20 more](img/forward20plus.png)


#### 5. [helppo - keskivaikea] Liikuta kaikkia viittä kilpikonnaa - perehdytys funktioihin

Haluamme liikuttaa ja kääntää viittä kilpikonnaamme.
Kuinka saisimme ne menemään eteenpäin 40 askelta?
Helpoin ratkaisu olisi kirjoittaa `(forward :name 40)`
viisi kertaa.

Mutta emme halua kirjoittaa lähes samaa funktiokutsua montaa kertaa.
Voisiko tämän tehdä helpommin? Kyllä vain.
Clojuressa tämän voi tehdä monella tavalla.
`doseq` funkti on yksi.

- 5.1 liikuta 5 kilpikonnaa eteenpäin käyttäen `doseq` funktiota. 

```clojure
clojurebridge-turtle.walk=> (doseq [n (turtle-names)] (forward n 40))
nil
```

![five turtles move](img/five-turtles-move.png)


- 5.2 [bonus] `map` funktiota käyttäen (ns. higher order-funktio)

```clojure
clojurebridge-turtle.walk=> (map #(forward % 40) (turtle-names))
({:trinity {:length 40}} {:neo {:length 40}} {:oracle {:length 40}} {:cypher {:length 40}} {:morpheus {:length 40}})
```


- 5.3 tilt and forward them by `doseq`s

```clojure
clojurebridge-turtle.walk=> (doseq [n (turtle-names)] (right n 60))
nil
clojurebridge-turtle.walk=> (doseq [n (turtle-names)] (forward n 30))
nil
```

![more moves](img/five-turtles-more-move.png)


- 5.4 [bonus] put two `doseq`s in one

```clojure
clojurebridge-turtle.walk=> (doseq [n (turtle-names)]
                       #_=> (right n 60)
                       #_=> (forward n 30))
nil
```

- 5.5 [bonus] using `map` (higher order function) and `juxt` functions

```clojure
clojurebridge-turtle.walk=> (map (juxt #(right % 60) #(forward % 30)) (turtle-names))
([{:trinity {:angle 60}} {:trinity {:length 30}}]
[{:neo {:angle 60}} {:neo {:length 30}}]
[{:oracle {:angle 60}} {:oracle {:length 30}}]
[{:cypher {:angle 60}} {:cypher {:length 30}}]
[{:morpheus {:angle 60}} {:morpheus {:length 30}}])
```


#### 6. [easy - intermediate] Write a function that adds turtles

While playing around with turtles, we may mess up their movements.
The `(init)` command makes everything clean up and back to the initial state.
It is a good command, but again, we need to repeat `(add-turtle name)`
command many times to get five turtles.

We want something. Yes, we can define our own function for that.
Once the function is defined, we can add five turtles by a single
function call anytime.


- 6.1 define a function to add three turtles and a turtle with the name :neo

Since we already have :trinity, we are going to add four turtles.
Let's name them, :neo, :oracle, :cypher, :morpheus.
Once all are added, we will get five turtles in total.

```clojure
;; function definition
(defn add-four-turtles
  []
  (let [names [:neo :oracle :cypher :morpheus]]
    (init)
    (dotimes [i (count names)] (add-turtle (nth names i)))))

;; usage of the five-turtles function
(add-four-turtles)

;; check turtle names
(turtle-names)
```

![add four turtles](img/add-four-turtles.png)

- 6.2 [bonus] add a parameter to `add-four-turtles` function so that
each turtle can take specified name.

Our `add-four-turtles` function uses hard-coded names for turtles.
Instead, let's enjoy a freedom to choose any name for them without
rewriting the function.

Now, we are going to write **multi-arity function**. The arity means a
number of arguments the function takes. For example, our
`add-four-turtle` function so far takes zero argument only. If we add
`add-four-turtle` function which takes one argument, the function
turns to a multi-arity function. This is because the function takes
zero or one argument.


```clojure
;; function definition
(defn add-four-turtles
  ([] (add-four-turtles [:neo :oracle :cypher :morpheus]))
  ([names]
    (init)
    (dotimes [i (count names)] (add-turtle (nth names i)))))

;; usage example
(add-four-turtles)                                 ;; uses predefined names
(add-four-turtles [:taylor :tess :tiffany :tracy]) ;; uses given names

;; check turtle names
(turtle-names)
```

Look at the multi-arity function above once more. The
`add-four-turtles` function doesn't need to have `four` in its name.
The number of turtles can be any. If you list two names in the vector,
the function will add two turtles.


- 6.3 [bonus] [exercise] make `add-four-turtles` to add exactly four turtles

We can add a check so that only when four names are provided, the
function adds four turtles; otherwise does nothing.

Hint: function body will look like:

```clojure
(if (= 4 (count names))
  ....blah blah blah...)
```

- 6.4 side note

Suppose you have already five turtles and continue using the same
turtles. But, you want to clean up all lines and make them home
position. In such a case, a combination of `clean-all` and
`home-all` gives the same effect as `add-four-turtles` function.

```clojure
;; deletes all lines
(clean-all)

;; moves all turtles back to the home position
(home-all)

;; check turtles' names
(turtle-names)
```

#### 7. [intermediate - difficult] Write a function that tilts five turtles in different directions

Next, we want to tilt five turtles' heads in different angles so that
we can see their move well. For example, :trinity 0, :neo 45, :oracle
90, :cypher 135, :morpheus 180.
This function will be no more straightforward like we wrote so far.
Since we need two kinds of parameters at the same time, name and
angle, while previous functions used only one kind of parameter.

Again, Clojure has many ways to do this.

- 7.1 using `doseq` with a little tweak

Let's think how we can use `doseq` here also.

- 7.1.1 put two parameters in one hash map

If we put two-parameter pair together to form one, we can use `doseq` like previous examples.

```clojure
;; put turtle names and digits together and create a hash map
clojurebridge-turtle.walk=> (def m (zipmap (turtle-names) (range 0 (count (turtle-names)))))
#'clojurebridge-turtle.walk/m
clojurebridge-turtle.walk=> m
{:trinity 0, :neo 1, :oracle 2, :cypher 3, :morpheus 4}

;; see each part is doing what
clojurebridge-turtle.walk=> (count (turtle-names))
5
clojurebridge-turtle.walk=> (range 0 5)
(0 1 2 3 4)
clojurebridge-turtle.walk=> (zipmap [:a :b] [0 1])
{:a 0, :b 1}
```

- 7.1.2 use `doseq` to iterate hash map

```clojure
;; using m which is created by zipmap
(doseq [t m] (right (first t) (* (second t) 45)))

;; again, see each part is doing what

;; see how doseq uses m
clojurebridge-turtle.walk=> (doseq [t m] (prn t))
[:trinity 0]
[:neo 1]
[:oracle 2]
[:cypher 3]
[:morpheus 4]
nil

;; see first and second function do
clojurebridge-turtle.walk=> (doseq [t m] (prn (first t) (second t)))
:trinity 0
:neo 1
:oracle 2
:cypher 3
:morpheus 4
nil

;; see what (* (second t) 45) does
clojurebridge-turtle.walk=> (doseq [t m] (prn (first t) (* (second t) 45)))
:trinity 0
:neo 45
:oracle 90
:cypher 135
:morpheus 180
nil
```

- 7.1.3 put hash map creation and `doseq` in one function

```clojure
;; function definition
(defn tilt-turtles
  []
  (let [m (zipmap (turtle-names) (range 0 (count (turtle-names))))]
    (doseq [t m] (right (first t) (* (second t) 45)))))

;; usage
(tilt-turtles)
```

![tilt turtles](img/tilt-turtles.png)


- 7.2 [bonus] map function (higher order function) is another way to do

Using `map` function, tilt-turtles function can be written a single
line as in below.

```clojure
clojurebridge-turtle.walk=> (map #(right % (* %2 45)) (turtle-names) (range 0 (count (turtle-names))))
({:trinity {:angle 0}} {:neo {:angle 45}} {:oracle {:angle 90}}
{:cypher {:angle 135}} {:morpheus {:angle 180}})
```

- 7.3 [bonus] recursion is another way to do

```clojure
;; function definition
(defn tilt-turtles-loop
  []
  (loop [t nil
         n (turtle-names)
         s 0]
    (if (empty? n) :completed
        (recur (right (first n) (* s 45)) (rest n) (inc s)))))

;; usage
(tilt-turtles-loop)
```

#### 8. [intermediate] Write a function that moves five turtles forward, right and forward again

We got five turtles, made their heads tilted in different directions.
It's time to move all those five turtles, forward, right and forward.

- 8.1 put three movements in one `doseq`

Like we tried in 5.4, only one `doseq` works for the three movements.

```clojure
(doseq [n (turtle-names)]
  (forward n 60)
  (right n 90)
  (forward n 50))
```

![move turtles](img/move-turtles.png)


- 8.2 change parameters of forwards and right

Putting three movements in one `doseq` is nice, but what if we want to
change parameters?
For example, forward 100, right 150, then forward 60, or
forward 50, right 30, and forward 60.

Defining a function that takes parameters is the way to do.

```clojure
;; function definition
(defn doseq-with-params
  [len1 len2 angle]
  (doseq [n (turtle-names)]
    (forward n len1)
    (right n angle)
    (forward n len2)))

;; usage example
(home-all)
(clean-all)
(tilt-turtles)

(doseq-with-params 100 60 150)
```

![params 100 60 150](img/parameters-100-60-150.png)


Try various parameters.


#### 9. [intermediate] put whole stuff in a function

So far, we added four turtles and got five in total.
Each turtle's head was tilted in a different direction.
Finally, five turtles walked forward, changed heads by some degrees,
then walked again.
There were three steps.

We've already learned writing a function makes things put into one, and
repeating the same sequence makes very easy.

- 9.1 write a function so that all three steps can be done by only one function.

```clojure
;; definition of walk-five-turtles functions
(defn walk-five-turtles
  [names len1 len2 angle]
  (add-four-turtles names)
  (tilt-turtles)
  (doseq-with-params len1 len2 angle))

;; usage examples
(walk-five-turtles [:taylor :tess :tiffany :tracy] 10 60 90)
```

![walk five turtles](img/walk-five-turtles.png)


- 9.2 [bonus] use hash map as a function parameter

As a number of function parameters increases, it goes confusing.
For example, if we misunderstand the order of len2 and angle, turtles
may go beyond the boundary.

To solve this problem, hash map is often used as a function parameter.
The parameters in the hash map are picked up using destructuring.
It is handy.


```clojure
;; function definition
(defn walk-five-turtles-map
  [{:keys [names len1 len2 angle]}]
  (add-four-turtles names)
  (tilt-turtles)
  (doseq-with-params len1 len2 angle))


;; usage example
(walk-five-turtles-map {:names [:taylor :tess :tiffany :tracy] :len1 10 :len2 60 :angle 90})
```

If we look at the usage example, it's clear what parameter goes where.



#### #. side note: forward 480 and 48 times of forward 10

Going forward 480 and repeating 48 times of forward 10 are not the
same for the turtles.
To see the difference, let define an utility function, `leftmost` first.
This function moves a turtle to the leftmost position.

```clojure
;; function definition

(defn leftmost
  [n]
  (home n)
  (clean n)
  (left n 90)
  (forward n 240)
  (clean n)
  (right n 180))
```

Once the turtle moves to the leftmost position, try two types of forwards.

```clojure
clojurebridge-turtle.walk=> (leftmost :trinity)
{:trinity {:angle 180}}

;; forward 480
clojurebridge-turtle.walk=> (forward :trinity 480)
{:trinity {:length 480}}
```

![forward 480](img/forward480.png)

```clojure
clojurebridge-turtle.walk=> (leftmost :trinity)
{:trinity {:angle 180}}

;; 48 times of forward 10
clojurebridge-turtle.walk=> (dotimes [i 48] (forward :trinity 10))
nil
```
![forward 48*10](img/forward48times10.png)


Both walked the turtle from the leftmost to rightmost position,
however, the colors are not the same.
The stroke's color is calculated from the x, y values of the endpoint.
Forwarding 480 has only one endpoint, while forwarding 48 times has 48
endpoints.
This means the turtle changed the color 48 times.




License
-------
<a rel="license"
href="http://creativecommons.org/licenses/by/4.0/deed.en_US"><img
alt="Creative Commons License" style="border-width:0"
src="http://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br
/><span xmlns:dct="http://purl.org/dc/terms/"
href="http://purl.org/dc/dcmitype/Text" property="dct:title"
rel="dct:type">ClojureBridge Curriculum</span> by <span
xmlns:cc="http://creativecommons.org/ns#"
property="cc:attributionName">ClojureBridge</span> is licensed under a
<a rel="license"
href="http://creativecommons.org/licenses/by/4.0/deed.en_US">Creative
Commons Attribution 4.0 International License</a>.
