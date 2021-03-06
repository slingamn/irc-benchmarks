# irc-benchmark

This repo holds benchmarks comparing various IRC libraries, all of which implement IRCv3 tags.

- [jakebailey/irc](https://github.com/jakebailey/irc), my IRC library. In the encoding tests,
`WriteTo` indicates that the output was directly written to an `io.Writer`
rather than creating a temporary buffer.
- [jakebailey/ircold](https://github.com/jakebailey/ircold), a fork of `MorpheusXAUT/irc`, which is a fork of
`github.com/sorcix/irc` (all to add IRCv3 tags).
- [sorcix/irc](https://github.com/sorcix/irc), the `ircv3.2-tags` branch.
- [fluffle/goirc/client](https://github.com/fluffle/goirc/client), a handler-based IRC library.
- [thoj/go-ircevent](https://github.com/thoj/go-ircevent), an event-based IRC library. This doesn't
actually export the message parsing function, so `go-forceexport` was used
to find a function pointer rather than fork the project.
- [goshuirc/irc-go/ircmsg](https://github.com/goshuirc/irc-go/ircmsg), an IRC message handler lib.
- [gempir/go-twitch-irc](https://github.com/gempir/go-twitch-irc), an IRC lib specifically for Twitch. This
comparison is likely unfair because it includes extra functionailty around
extracting tags.

```
benchmark                                              iter        time/iter   bytes alloc          allocs
---------                                              ----        ---------   -----------          ------
BenchmarkParseSimple/jakebailey/irc-4              10000000     130.00 ns/op       32 B/op     1 allocs/op
BenchmarkParseSimple/jakebailey/ircold-4           10000000     223.00 ns/op      144 B/op     3 allocs/op
BenchmarkParseSimple/fluffle/goirc/client-4         5000000     367.00 ns/op      288 B/op     4 allocs/op
BenchmarkParseSimple/sorcix/irc-4                  10000000     223.00 ns/op      144 B/op     3 allocs/op
BenchmarkParseSimple/thoj/go-ircevent-4             5000000     356.00 ns/op      256 B/op     4 allocs/op
BenchmarkParseSimple/goshuirc/irc-go/ircmsg-4       2000000     659.00 ns/op      336 B/op    11 allocs/op
BenchmarkParseSimple/gempir/go-twitch-irc-4        10000000     192.00 ns/op      400 B/op     3 allocs/op

BenchmarkParseTwitch/jakebailey/irc-4               1000000    1364.00 ns/op     1234 B/op     3 allocs/op
BenchmarkParseTwitch/jakebailey/ircold-4             300000    5689.00 ns/op     4015 B/op    64 allocs/op
BenchmarkParseTwitch/fluffle/goirc/client-4          300000    5894.00 ns/op     4159 B/op    65 allocs/op
BenchmarkParseTwitch/sorcix/irc-4                    300000    5755.00 ns/op     4015 B/op    64 allocs/op
BenchmarkParseTwitch/thoj/go-ircevent-4              300000    4700.00 ns/op     3071 B/op    23 allocs/op
BenchmarkParseTwitch/goshuirc/irc-go/ircmsg-4        200000   11177.00 ns/op     5873 B/op   155 allocs/op
BenchmarkParseTwitch/gempir/go-twitch-irc-4          300000    4789.00 ns/op     2601 B/op    34 allocs/op

BenchmarkParseEscaping/jakebailey/irc-4              500000    2608.00 ns/op     1553 B/op     9 allocs/op
BenchmarkParseEscaping/jakebailey/ircold-4           200000    7874.00 ns/op     4877 B/op    84 allocs/op
BenchmarkParseEscaping/fluffle/goirc/client-4        200000    8012.00 ns/op     4958 B/op    84 allocs/op
BenchmarkParseEscaping/sorcix/irc-4                  200000    7886.00 ns/op     4877 B/op    84 allocs/op
BenchmarkParseEscaping/thoj/go-ircevent-4            200000    6617.00 ns/op     3549 B/op    31 allocs/op
BenchmarkParseEscaping/goshuirc/irc-go/ircmsg-4      100000   17454.00 ns/op    10230 B/op   246 allocs/op
BenchmarkParseEscaping/gempir/go-twitch-irc-4        300000    5414.00 ns/op     3629 B/op    30 allocs/op

BenchmarkEncodeSimple/jakebailey/irc-4             10000000     118.00 ns/op       48 B/op     1 allocs/op
BenchmarkEncodeSimple/jakebailey/irc_WriteTo-4     20000000     100.00 ns/op        0 B/op     0 allocs/op
BenchmarkEncodeSimple/jakebailey/ircold-4          10000000     138.00 ns/op      112 B/op     1 allocs/op
BenchmarkEncodeSimple/sorcix/irc-4                 10000000     139.00 ns/op      112 B/op     1 allocs/op
BenchmarkEncodeSimple/goshuirc/irc-go/ircmsg-4     10000000     155.00 ns/op      112 B/op     1 allocs/op

BenchmarkEncodeTwitch/jakebailey/irc-4              2000000     842.00 ns/op      352 B/op     1 allocs/op
BenchmarkEncodeTwitch/jakebailey/irc_WriteTo-4      2000000     739.00 ns/op        0 B/op     0 allocs/op
BenchmarkEncodeTwitch/jakebailey/ircold-4           1000000    1224.00 ns/op     1190 B/op     3 allocs/op
BenchmarkEncodeTwitch/sorcix/irc-4                  1000000    1220.00 ns/op     1178 B/op     3 allocs/op
BenchmarkEncodeTwitch/goshuirc/irc-go/ircmsg-4      1000000    1671.00 ns/op     1244 B/op     4 allocs/op

BenchmarkEncodeEscaping/jakebailey/irc-4            1000000    1375.00 ns/op      480 B/op     1 allocs/op
BenchmarkEncodeEscaping/jakebailey/irc_WriteTo-4    1000000    1251.00 ns/op        0 B/op     0 allocs/op
BenchmarkEncodeEscaping/jakebailey/ircold-4         1000000    1370.00 ns/op     1365 B/op     4 allocs/op
BenchmarkEncodeEscaping/sorcix/irc-4                1000000    1385.00 ns/op     1306 B/op     4 allocs/op
BenchmarkEncodeEscaping/goshuirc/irc-go/ircmsg-4     500000    2415.00 ns/op     1612 B/op     8 allocs/op
```

`github.com/fluffle/goirc/client`, `github.com/thoj/go-ircevent` and `github.com/gemir/go-twitch-irc`
are omitted from the encoding tests,
as they do not support encoding of their message types, only sending strings (with or without helpers
for common commands).


Formatted using [prettybench](https://github.com/cespare/prettybench).