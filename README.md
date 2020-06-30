This project is fork of [doodledood/elm-split-pane](https://github.com/doodledood/elm-split-pane) by [doodledood](https://github.com/doodledood).

Copyright and license of the original project can be found [here](https://github.com/doodledood/elm-split-pane/blob/master/LICENCE).

# Split Pane

A split pane for Elm 0.19.

Embed two views beside each other with a resizable splitter in between.

## Usage Rules

  - Always put `SplitPane.State` in your model.
  - Never put _any_ `Config` in your model.
  - Don't forget to register subscriptions for dragging to work.
  - To control the pane's size place the pane inside a container and give the container a size

Design inspired by [elm-sortable-table](https://github.com/evancz/elm-sortable-table/).

Read about why these usage rules are good rules [here](https://github.com/evancz/elm-sortable-table/tree/1.0.0#usage-rules).

## Installation

```
elm install whale9490/elm-split-pane
```

## Examples

[Examples code](https://github.com/whale9490/elm-split-pane/tree/master/examples)

To run examples, clone this repo and,

```
cd examples
elm reactor
```

## Basic Usage

Use it just like any other TEA component.

```elm

module Simple exposing (..)

import Browser
import Html exposing (..)
import Html.Attributes exposing (src, style)
import SplitPane exposing (Orientation(..), ViewConfig, createViewConfig)


main : Program () Model Msg
main =
    Browser.element
        { update = update
        , init = init
        , subscriptions = subscriptions
        , view = view
        }



-- MODEL


type alias Model =
    { pane : SplitPane.State
    }


type Msg
    = PaneMsg SplitPane.Msg



-- INIT


init : () -> ( Model, Cmd a )
init _ =
    ( { pane = SplitPane.init Horizontal
      }
    , Cmd.none
    )



-- UPDATE


update : Msg -> Model -> ( Model, Cmd a )
update msg model =
    case msg of
        PaneMsg paneMsg ->
            ( { model | pane = SplitPane.update paneMsg model.pane }, Cmd.none )



-- VIEW


view : Model -> Html Msg
view model =
    div
        [ style "width" "800px"
        , style "height" "600px"
        ]
        [ SplitPane.view viewConfig firstView secondView model.pane ]


viewConfig : ViewConfig Msg
viewConfig =
    createViewConfig
        { toMsg = PaneMsg
        , customSplitter = Nothing
        }


firstView : Html a
firstView =
    img [ src "http://4.bp.blogspot.com/-s3sIvuCfg4o/VP-82RkCOGI/AAAAAAAALSY/509obByLvNw/s1600/baby-cat-wallpaper.jpg" ] []


secondView : Html a
secondView =
    img [ src "http://2.bp.blogspot.com/-pATX0YgNSFs/VP-82AQKcuI/AAAAAAAALSU/Vet9e7Qsjjw/s1600/Cat-hd-wallpapers.jpg" ] []



-- SUBSCRIPTIONS


subscriptions : Model -> Sub Msg
subscriptions model =
    Sub.map PaneMsg <| SplitPane.subscriptions model.pane


```