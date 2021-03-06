GeoTools Feature Types
======================

A ``SimpleFeatureType`` defines a GeoTools schema, and consists of an array of well-known attributes. GeoMesa
supports all of the standard GeoTools attribute types, as well as some additional ones. When creating
a ``SimpleFeatureType`` for use in GeoMesa, be sure to use the provided classes, instead of the standard
GeoTools ``DataUtilities``:

.. tabs::

    .. code-tab:: java

        import org.locationtech.geomesa.utils.interop.SimpleFeatureTypes;

        SimpleFeatureTypes.createType("example", "name:String,dtg:Date,*geom:Point:srid=4326");

    .. code-tab:: scala

        import org.locationtech.geomesa.utils.geotools.SimpleFeatureTypes

        SimpleFeatureTypes.createType("example", "name:String,dtg:Date,*geom:Point:srid=4326")

Available Types
---------------

================== ============================================== =========
Attribute Type     Binding                                        Indexable
================== ============================================== =========
String             java.lang.String                               Yes
Integer            java.lang.Integer                              Yes
Double             java.lang.Double                               Yes
Long               java.lang.Long                                 Yes
Float              java.lang.Float                                Yes
Boolean            java.lang.Boolean                              No
UUID               java.util.UUID                                 No
Date               java.util.Date                                 Yes
Timestamp          java.sql.Timestamp                             Yes
Point              com.vividsolutions.jts.geom.Point              Yes
LineString         com.vividsolutions.jts.geom.LineString         Yes
Polygon            com.vividsolutions.jts.geom.Polygon            Yes
MultiPoint         com.vividsolutions.jts.geom.MultiPoint         Yes
MultiLineString    com.vividsolutions.jts.geom.MultiLineString    Yes
MultiPolygon       com.vividsolutions.jts.geom.MultiPolygon       Yes
GeometryCollection com.vividsolutions.jts.geom.GeometryCollection Yes
Geometry           com.vividsolutions.jts.geom.Geometry           Yes
List[A]            java.util.List<A>                              Yes
Map[A,B]           java.util.Map<A, B>                            No
Bytes              byte[]                                         No
================== ============================================== =========

Notes
^^^^^

* Only a single geometry-type attribute may be indexed. It will be included in the primary spatial
  and spatio-temporal indices.
* The primary date-type attribute will be used in the spatio-temporal index. It may also be indexed
  separately in an attribute index.
* Non-geometry types (including dates) may be indexed as separate attribute indices. For details, see
  :ref:`attribute_indices`.
* Container types (List and Map) must be parameterized with non-container types from the above table.
* List types may be indexed, but querying a list type may be slow due to deduplication.
