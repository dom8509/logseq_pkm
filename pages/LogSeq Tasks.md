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
- #+BEGIN_QUERY
  {:title "📅 NEXT"
    :query [:find (pull ?b [*])
            :in $ ?start ?next
            :where
            (task ?b #{"NOW" "LATER" "TODO" "DOING"})
            (between ?b ?start ?next)]
    :inputs [:today :10d-after]
    :collapsed? false}
  #+END_QUERY
- #+BEGIN_QUERY
     {:title "🟠 SLIPPING"
    :query [:find (pull ?b [*])
            :in $ ?start ?today
            :where
            (task ?b #{"NOW" "LATER" "TODO" "DOING"})
            (between ?b ?start ?today)]
    :inputs [:7d :today]
    :result-transform (fn [result]
                        (sort-by (fn [h]
                                   (get h :block/created-at)) result))
    :collapsed? true}
  #+END_QUERY
- #+BEGIN_QUERY
  {:title "🔴 STALLED"
    :query [:find (pull ?h [*])
            :in $ ?start ?today
            :where
            (task ?b #{"NOW" "LATER" "TODO" "DOING"})
            (between ?b ?start ?today)]
    :inputs [:56d :8d]
    :result-transform (fn [result]
                        (sort-by (fn [h]
                                   (get h :block/created-at)) result))
    :collapsed? true}
   ]}
  #+END_QUERY
- #+BEGIN_QUERY
  {:title "next 7 days' deadline or schedule"
    :query [:find (pull ?block [*])
            :in $ ?start ?next
            :where
            (or
              [?block :block/scheduled ?d]
              [?block :block/deadline ?d])
            [(> ?d ?start)]
            [(< ?d ?next)]]
    :inputs [:today :7d-after]
    :collapsed? false}
  #+END_QUERY
- #+BEGIN_QUERY
  {:title "🔴 STALLED"
    :query [:find (pull ?attr [*])
            :where
            [?e ?attr]
    :collapsed? true}
   ]}
  #+END_QUERY
-