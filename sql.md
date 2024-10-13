1. UnionAll vs Union
   Union is slow as it only provides distinct elements and union all is fast because it gives duplicates as well.
   Select requester_id from RequestAccepted
   Union all
   Select accepter_id from RequestAccepted
