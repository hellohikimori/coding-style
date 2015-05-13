#Stylus coding style

## Style
Use stylus bracket-less and colon-less style for more clarity.

```stylus
/* Don't */
.stuff {
  color: blue;
}

/* Do */
.stuff
  color blue
```

## Structure
Use this `BEM` notation :

* PascalCase for module name.
* `--` to announce a modifier.
* `-` to announce a child.
* `.is` + camelCase to announce a state.

Use cascade with & on two levels maximum.

```stylus
.MyButton {
  position: relative;
  opacity: 0.5;

  &--alert {
    color: red;
  }

  &-border {
    border: 1px solid white;
  }

  &.isActive {
    opacity: 1;
  }
}
```
