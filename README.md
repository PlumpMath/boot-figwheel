# boot-figwheel

####Current version:
[![Clojars Project](https://clojars.org/ajchemist/boot-figwheel/latest-version.svg)](http://clojars.org/ajchemist/boot-figwheel)

#### Usage
[](dependency)
```clojure
[ajchemist/boot-figwheel "0.3.7-1"] ;; latest release
```
[](/dependency)

You don't need to add `figwheel`,`figwheel-sidecar` or of course `lein-figwheel`
to your dependency.

[](require)
```clojure
(require '[boot-figwheel :refer [figwheel run-figwheel stop-figwheel start-figwheel]])
```
[](/require)

```clojure
(task-options!
 figwheel {:figwheel-config
           (let [p (rand-port)]
             {:builds [{:id "dev"
                        :source-paths ["src"]
                        :compiler (merge none-opts
                                         {:main "adzerk.boot-cljs-repl"
                                          :output-to "target/app.js"
                                          :output-dir "target/out"
                                          :asset-path "out"})
                        :figwheel {:websocket-url (format "ws://localhost:%d/figwheel-ws" p)
                                   :build-id "dev"
                                   :on-jsload "<<ns.core>>.main"
                                   :heads-up-display true
                                   :autoload true
                                   :debug false}}]
              :figwheel-server {:repl true
                                :server-port p
                                :http-server-root "target"
                                :css-dirs ["target"]
                                :open-file-command "emacsclient"}})})
```

```clojure
(deftask dev []
  (set-env! :source-paths #(into % ["src"]))
  (comp (figwheel) (cljs-repl) (wait)))
```

Boot `:source-paths` env get passthru figwheel task internal state at figwheel
pod generation time. So it can work with `cljs-repl`.

#### Limitation

Figwheel has own fileset watcher. It can't be cooperated with boot-clj `watch`
task. So you have to edit a file in `target-path` directly, if you want to use
features like figwheel css live reloading, etc.

