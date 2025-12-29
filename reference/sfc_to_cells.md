# Convert sf geometry to H3 Cells

Given a vector of sf geometries (class `sfc`) create a list of `H3`
vectors. Each list element contains the vector of H3 cells that cover
the geometry.

## Usage

``` r
sfc_to_cells(x, resolution, containment = "intersect")
```

## Arguments

- x:

  for
  [`h3_from_points()`](https://extendr.github.io/h3o/reference/H3.md) an
  object of class `sfc_POINT`. For
  [`h3_from_strings()`](https://extendr.github.io/h3o/reference/H3.md) a
  character vector of H3 index IDs. For
  [`h3_from_xy()`](https://extendr.github.io/h3o/reference/H3.md) a
  numeric vector of longitudes.

- resolution:

  an integer indicating the H3 cell resolution. Must be between 0 and 15
  inclusive.

- containment:

  default `"intersect"`. Must be one of `"intersect"`, `"centroid"`, or
  `"boundary"`. See details.

## Value

An H3 vector.

## Details

Note, use
[`flatten_h3()`](https://extendr.github.io/h3o/reference/H3.md) to
reduce the list to a single vector.

The [Containment
Mode](https://docs.rs/h3o/0.4.0/h3o/geom/enum.ContainmentMode.html)
determines if an H3 cell should be returned.

- `"centroid"` returns every cell whose centroid are contained inside of
  a polygon. This is the fastest option but may not cover the entire
  polygon.

- `"boundary"` this returns the cells which are completely contained by
  the polygon. Much of a polygon might not be covered using this
  approach.

- `"intersect"` ensures that a polygon is entirely covered. If an H3
  cell comes in contact with the polygon it will be returned. This is
  the default.

- `"contains"` behaves the same as `"intersect"`, but also handles the
  case where the geometry is being covered by a cell without
  intersecting with its boundaries. In such cases, the covering cell is
  returned.

## Examples

``` r
if (interactive() && rlang::is_installed("sf")) {
  nc <- sf::st_read(system.file("shape/nc.shp", package = "sf"), quiet = TRUE)
  geo <- sf::st_geometry(nc)
  cells <- sfc_to_cells(geo, 5)

  head(cells)

  plot(flatten_h3(cells))
}
```
