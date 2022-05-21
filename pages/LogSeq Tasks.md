query-table:: false
#+BEGIN_QUERY
{:title "All tasks"
 :query [:find (pull ?b [*])
         :where
         [?b :block/marker _]]}
#+END_QUERY

- #+BEGIN_QUERY
  {:title "🟢 ACTIVE"
    :query [:find (pull ?b [*])
            :in $ ?start ?today
            :where
            (task ?b #{"NOW" "DOING"})
            (between ?b ?start ?today)]
    :inputs [:14d :today]
    :result-transform (fn [result]
                        (sort-by (fn [h]
                                   (get h :block/priority "Z")) result))
    :collapsed? false}
  #+END_QUERY
- #+BEGIN_QUERY
   {:title "⚠️ OVERDUE"
    :query [:find (pull ?b [*])
            :in $ ?start ?today
            :where
            (task ?b #{"NOW" "LATER" "TODO" "DOING"})
            (between ?b ?start ?today)]
    :inputs [:56d :today]
    :collapsed? false}
  #+END_QUERY