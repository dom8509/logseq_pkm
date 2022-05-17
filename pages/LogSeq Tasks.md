- #+BEGIN_QUERY
  {:title " Scheduled TODOs"
  :query [:find (pull ?b [*])
  :where
  [?b :block/scheduled ?d]
  [?b :block/marker ?marker]
  [(not= ?d nil)]
  [(contains? #{"NOW" "LATER" "DOING" "TODO"} ?marker)]]
  :collapsed? false}
  #+END_QUERY