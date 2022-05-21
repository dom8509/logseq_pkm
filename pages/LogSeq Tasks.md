query-table:: false
#+BEGIN_QUERY
{:title "All tasks"
 :query [:find (pull ?b [*])
         :where
         [?b :block/marker _]]}
#+END_QUERY

- #+BEGIN_QUERY
  {:title "Journal blocks in last 7 days with a page reference of datalog"
   :query [:find (pull ?b [*])
           :in $ ?start ?today ?tag
           :where
           (between ?b ?start ?today)
           (page-ref ?b ?tag)]
   :inputs [:7d-before :today "datalog"]}
  #+END_QUERY