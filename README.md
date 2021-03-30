mongostore
==========

[Gorilla's Session](http://www.gorillatoolkit.org/pkg/sessions) store implementation with MongoDB using the legacy (unmantained) [mgo](https://github.com/go-mgo/mgo) library

This repository is meant to give some backwards compatibility to some projects that can't upgrade to other MongoDB driver at the moment. If you need/want to use the more up to date [mgo](https://labix.org/v2/mgo) library check the [original](https://github.com/kidstuff/mongostore) library from where this one derives.

## Requirements

Depends on the unmantained [mgo](https://github.com/go-mgo/mgo) library.

## Installation

    go get github.com/padiazg/mongostore

## Documentation

Available on [godoc.org](http://www.godoc.org/github.com/padiazg/mongostore).

### Example
```go
    func foo(rw http.ResponseWriter, req *http.Request) {
        // Fetch new store.
        dbsess, err := mgo.Dial("localhost")
        if err != nil {
            panic(err)
        }
        defer dbsess.Close()

        store := mongostore.NewMongoStore(dbsess.DB("test").C("test_session"), 3600, true,
            []byte("secret-key"))

        // Get a session.
        session, err := store.Get(req, "session-key")
        if err != nil {
            log.Println(err.Error())
        }

        // Add a value.
        session.Values["foo"] = "bar"

        // Save.
        if err = sessions.Save(req, rw); err != nil {
            log.Printf("Error saving session: %v", err)
        }

        fmt.Fprintln(rw, "ok")
    }
```
