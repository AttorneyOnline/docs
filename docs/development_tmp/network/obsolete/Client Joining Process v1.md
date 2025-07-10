# Client Joining Process v1

This was used before the v2 of the joining process was created.

It is typicaly much slower than v2, as it uses pagination for fetching content.

1. Client sends `askchar2`
2. Server sends `CI`, with `page_number` as `n+1` (first `n+1` is `0`)
3. Client sends `AN`, with `page_number` as `n+1` (first `n+1` is `1`)
4. Steps 2. and 3. are repeated until the server has no more pages of characters,, then it moves on to 5.
5. Server sends `EI` with `page_number` as `n+1` (first `n+1` is `1`) (interestingly, evidence starts at page number 1 and not 0)
6. Client sends `AE` with `page_number` as `n+1` (first `n+1` is `2`)
7. Steps 6. and 7. are repeated until the server has no more pages of evidence, then it moves on to 8.
8. Server sends `EM` with `page_number` as `n+1` (first `n+1` is `0`)
9. Client sends `EM` with `page_number` as `n+1` (first `n+1` is `1`)
10. Steps 8. and 9. are repeated until the server has no more pages of music, then it moves on to 11.
11. Server sends `CharsCheck`

When the client has received `CharsCheck`, it is considered joined.
