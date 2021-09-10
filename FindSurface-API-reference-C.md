

# Data Structures



## Typedefs



### Context

| Type                              | Name | Description                                           |
| --------------------------------- | ---- | ----------------------------------------------------- |
| `FIND_SURFACE_CONTEXT` (`void *`) | N/A  | An opaque type represents the context of FindSurface. |



## Enumerations



### `FS_ERROR`

| Enum                     | Value | Description                                                  |
| ------------------------ | ----- | ------------------------------------------------------------ |
| `FS_NO_ERROR`            | 0     | Found a geometry surface without any error.                  |
| `FS_OUT_OF_MEMORY`       | -1    | Failed to allocate memory blocks in the context due to the out-of-memory issue. |
| `FS_INVALID_OPERATION`   | -2    | Failed to operate due to the invalid state of the context.   |
| `FS_INVALID_VALUE`       | -3    | Failed to operate due to the invalid values of parameters in the context. |
| `FS_NOT_FOUND`           | -100  | Not error but found no surfaces.                             |
| `FS_UNACCEPTABLE_RESULT` | -101  | Reserved for debugging.                                      |



### `FS_FEATURE_TYPE`

| Enum               | Value | Description                                                  |
| ------------------ | ----- | ------------------------------------------------------------ |
| `FS_TYPE_ANY`      | 0     | Used for input values to search for one of the five types below. |
| `FS_TYPE_PLANE`    | 1     | Plane                                                        |
| `FS_TYPE_SPHERE`   | 2     | Sphere                                                       |
| `FS_TYPE_CYLINDER` | 3     | Cylinder                                                     |
| `FS_TYPE_CONE`     | 4     | Cone                                                         |
| `FS_TYPE_TORUS`    | 5     | Torus                                                        |
| `FS_TYPE_NONE`     | 6     | Indicates that no surfaces found. Not used in the C library of FindSurface. |



### `FS_SEARCH_LEVEL`

| Enum                               | Value | Description              |
| ---------------------------------- | ----- | ------------------------ |
| `FS_LEVEL_0` (`FS_LEVEL_OFF`)      | 0     | Search level 0           |
| `FS_LEVEL_1` (`FS_LEVEL_MODERATE`) | 1     | Search level 1           |
| `FS_LEVEL_2`                       | 2     | Search level 2           |
| `FS_LEVEL_3`                       | 3     | Search level 3           |
| `FS_LEVEL_4`                       | 4     | Search level 4           |
| `FS_LEVEL_5` (`FS_LEVEL_DEFAULT`)  | 5     | Search level 5 (default) |
| `FS_LEVEL_6`                       | 6     | Search level 6           |
| `FS_LEVEL_7`                       | 7     | Search level 7           |
| `FS_LEVEL_8`                       | 8     | Search level 8           |
| `FS_LEVEL_9`                       | 9     | Search level 9           |
| `FS_LEVEL_10` (`FS_LEVEL_RADICAL`) | 10    | Search level 10          |



### `FS_SMART_CONVERSION_OPTION`

| Enum flag                  | Value    | Description                                                  |
| -------------------------- | -------- | ------------------------------------------------------------ |
| `FS_SCO_NONE`              | 0        | No smart conversion.                                         |
| `FS_SCO_CONE_TO_CYLINDER`  | (1 << 0) | Cones with the vertex angle of zero is converted to cylinders. |
| `FS_SCO_TORUS_TO_SPHERE`   | (1 << 1) | Tori with the mean radius of zero (degenerated), is converted to spheres (double-covered). |
| `FS_SCO_TORUS_TO_CYLINDER` | (1 << 2) | Tori with the mean radius of infinite is converted to cylinders. |



## Structures



### `FS_FEATURE_RESULT`

| Type                        | Name   | Description                                                  |
| --------------------------- | ------ | ------------------------------------------------------------ |
| `FS_FEATURE_TYPE`           | `type` | The feature type of the resulted surface. This value could be one of `FS_TYPE_PLANE`, `FS_TYPE_SPHERE`, `FS_TYPE_CYLINDER`, `FS_TYPE_CONE` and `FS_TYPE_TORUS`. |
| `float`                     | `rms`  | RMS error of the resulted surface. Refer to [here][RMS] for the meaning of this value. |
| [`union`](#Anonymous-Union) | N/A    | An anonymous union containing various information about the resulted surface. |



### Anonymous Union

| Type                              | Name             | Description                                                  |
| --------------------------------- | ---------------- | ------------------------------------------------------------ |
| `float[14]`                       | `reserved`       | A placeholder having the maximum size among the structures below. |
| [`struct`](#Anonymous-Structures) | `plane_param`    | An anonymous structure containing information about the resulted surface if it is a plane. |
| [`struct`](#Anonymous-Structures) | `sphere_param`   | An anonymous structure containing information about the resulted surface if it is a sphere. |
| [`struct`](#Anonymous-Structures) | `cylinder_param` | An anonymous structure containing information about the resulted surface if it is a cylinder. |
| [`struct`](#Anonymous-Structures) | `cone_param`     | An anonymous structure containing information about the resulted surface if it is a cone. |
| [`struct`](#Anonymous-Structures) | `torus_param`    | An anonymous structure containing information about the resulted surface if it is a torus. |



### Anonymous Structures

**plane_param**:

| Type       | Name | Description                                                 |
| ---------- | ---- | ----------------------------------------------------------- |
| `float[3]` | `ll` | Coordinates of the lower-left bound of the resulted plane.  |
| `float[3]` | `lr` | Coordinates of the lower-right bound of the resulted plane. |
| `float[3]` | `ur` | Coordinates of the upper-right bound of the resulted plane. |
| `float[3]` | `ul` | Coordinates of the upper-left bound of the resulted plane.  |

**sphere_param**:

| Type       | Name | Description                                             |
| ---------- | ---- | ------------------------------------------------------- |
| `float[3]` | `c`  | Coordinates of the center point of the resulted sphere. |
| `float`    | r    | The radius of the resulted sphere.                      |

**cylinder_param**:

| Type       | Name | Description                                                  |
| ---------- | ---- | ------------------------------------------------------------ |
| `float[3]` | `b`  | Coordinates of the bottom-center point of the resulted cylinder. |
| `float[3]` | `t`  | Coordinates of the top-center point of the resulted cylinder. |
| `float`    | `r`  | The radius of the resulted cylinder.                         |

**cone_param**:

| Type       | Name | Description                                                  |
| ---------- | ---- | ------------------------------------------------------------ |
| `float[3]` | `b`  | Coordinates of the bottom-center point of the resulted cone. |
| `float[3]` | `t`  | Coordinates of the top-center point of the resulted cone.    |
| `float`    | `br` | The radius of the resulted cone at its bottom.               |
| `float`    | `tr` | The radius of the resulted cone at its top.                  |

**torus_param**:

| Type       | Name | Description                                            |
| ---------- | ---- | ------------------------------------------------------ |
| `float[3]` | `c`  | Coordinates of the center point of the resulted torus. |
| `float[3]` | `n`  | Coordinates of the axis vector of the resulted torus.  |
| `float`    | `mr` | The mean radius of the resulted torus.                 |
| `float`    | `tr` | The tube radius of the resulted torus.                 |



# API Functions



## Context



### `createFindSurface`

<dl>
  <dt>Signature</dt>
  <dd><code><a href="#FS_ERROR">FS_ERROR</a> createFindSurface(<a href="#Context">FIND_SURFACE_CONTEXT</a> *context)</code></dd>
  <dt>Summary</dt>
  <dd>Creates the FindSurface context object and returns it into the given pointer <code>context</code>.</dd>
  <dt>Return</dt>
  <dd>An error code of <a href="#FS_ERROR"><code>FS_ERROR</code></a> with value of <a href="#FS_ERROR"><code>FS_NO_ERROR</code></a> indicating if succeeded to create the context. Otherwise, error codes corresponding to the reason of the failure is returned.</dd>
  <dt>Possible Errors</dt>
  <dd><a href="#FS_ERROR"><code>FS_OUT_OF_MEMORY</code></a> in case that the system fails to allocate memory for the context. In this case, <code>context</code> will not be modified.</dd>
</dl>



### `cleanUpFindSurface`

<dl>
  <dt>Signature</dt>
  <dd><code>void cleanUpFindSurface(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code></dd>
  <dt>Summary</dt>
  <dd>Resets the <code>context</code> to the initial state. All parameters will be reset to their default value and the internal buffers allocated by the previous invocations of <a href="#setPointCloudFloat/Double"><code>setPointCloudFloat/Double</code></a> or <a href="#getInOutlierFlags"><code>getInOutlierFlags</code></a> will be released.</dd>
</dl>



### `releaseFindSurface`

<dl>
  <dt>Signature</dt>
  <dd><code>void releaseFindSurface(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code></dd>
  <dt>Summary</dt>
  <dd>Release the FindSurface <code>context</code>.</dd>
</dl>



## PointCloud



## `setPointCloudFloat/Double`

<dl>
  <dt>Signature</dt>
  <dd>
    <code><a href="#FS_ERROR">FS_ERROR</a> setPointCloudFloat(<a href="#context">FIND_SURFACE_CONTEXT</a> context, const void *pointer, unsigned int count, unsigned int stride)</code>
  </dd>
  <dd>
    <code><a href="#FS_ERROR">FS_ERROR</a> setPointCloudDouble(<a href="#context">FIND_SURFACE_CONTEXT</a> context, const void *pointer, unsigned int count, unsigned int stride)</code>
  </dd>
  <dt>Summary</dt>
  <dd>Sets input pointcloud data through the <code>pointer</code> to the data array, <code>count</code> of the points and <code>stride</code> of the point element in the data array. <code>stride</code> can be zero if the data array contains tightly-packed xyz values. In that case, the value is considered to be 12 for `float` array or 24 for `double` array. </dd>
  <dt>Return</dt>
  <dd>An error code of <code><a href="#FS_ERROR">FS_ERROR</a></code> with value of <code><a href="#FS_ERROR">FS_NO_ERROR</a></code> indicating if succeeded to create the context. Otherwise, error codes corresponding to the reason of the failure is returned.</dd>
  <dt>Possible Errors</dt>
  <dd><code><a href="#FS_ERROR">FS_OUT_OF_MEMORY</a></code> if the system fails to allocate memory for the input points.</dd> 
  <dd>
    <code><a href="#FS_ERROR">FS_INVALID_VALUE</a></code> if one of the following cases is true:
    <ul>
      <li><code>pointer</code> is <code>NULL</code>;</li>
      <li><code>count</code> is zero;</li>
    	<li><code>stride</code> is a non-zero value that is less than the stride of when the array contains tightly-packed xyz.</li>
    </ul>
  </dd>
  <dt>Note</dt>
  <dd>The invocation of this function with <code>pointer</code> is set to <code>NULL</code> or <code>count</code> is zero will be ignored silently.</dd>
  <dd>The points data given through <code>pointer</code> is copied to an internal buffer in the context. The buffer will be released and reallocated when new points is given to the context through this function.</dd>
</dl>



### `getPointCloudCount`

<dl>
  <dt>Signature</dt>
  <dd>
    <code>unsigned int getPointCloudCount(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code>
  </dd>
  <dt>Return</dt>
  <dd>The number of points given to the context previously.</dd>
</dl>




## Parameters

Refer to [here][PARAM] for the meanings of these parameters.



### setMeasurementAccuracy

<dl>
  <dt>Signature</dt>
  <dd>
    <code>void setMeasurementAccuracy(<a href="#context">FIND_SURFACE_CONTEXT</a> context, float accuracy)</code>
  </dd>
  <dt>Summary</dt>
  <dd>Sets measurement accuracy to the given <code>accuracy</code>. The value must be non-zero positive. The default value is zero.</dd>
</dl>


> Note: this parameter must be set manually before invoking FindSurface's algorithm since the default value will be considered to be invalid when the algorithm begins to operate.



### `getMeasurementAccuracy`

<dl>
  <dt>Signature</dt>
  <dd>
  	<code>float getMeasurementAccuracy(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code>
  </dd>
  <dt>Return</dt>
  <dd>The measurement accuracy value that is currently set to the context.</dd>
</dl>




### `setMeanDistance`

<dl>
  <dt>Signature</dt>
  <dd>
  	<code>void setMeanDistance(<a href="#context">FIND_SURFACE_CONTEXT</a> context, float distance)</code>
  </dd>
  <dt>Summary</dt>
  <dd>Sets mean distance to the given <code>distance</code>. The value must be non-zero positive. The default value is zero.</dd>
</dl>


> Note: this parameter must be set manually before invoking FindSurface's algorithm since the default value will be considered to be invalid when the algorithm begins to operate.



### `getMeanDistance`

<dl>
  <dt>Signature</dt>
  <dd>
    <code>float getMeanDistance(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code>
  </dd>
  <dt>Return</dt>
  <dd>The mean distance value that is currently set to the context.</dd>
</dl>




### `setRadialExpansion`

<dl>
  <dt>Signature</dt>
  <dd>
    <code>void setRadialExpansion(<a href="#context">FIND_SURFACE_CONTEXT</a> context, <a href="#FS_SEARCH_LEVEL">FS_SEARCH_LEVEL</a> level)</code>
  </dd>
  <dt>Summary</dt>
  <dd>Sets radial expasion to the given <code>level</code>. The value must be one of <code><a href="#FS_SEARCH_LEVEL">enum FS_SEARCH_LEVEL</a></code> values. The default value is <code><a href="#FS_SEARCH_LEVEL">FS_LEVEL_5</a></code>.</dd>
</dl>




### `getRadialExpansion`

<dl>
  <dt>Signature</dt>
  <dd><code><a href="#FS_SEARCH_LEVEL">FS_SEARCH_LEVEL</a> getRadialExpansion(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code></dd>
  <dt>Return</dt>
  <dd>The radial expansion value that is currently set to the context.</dd>
</dl>




### `setLateralExtension`

<dl>
  <dt>Signature</dt>
  <dd>
    <code>void setLateralExtension(<a href="#context">FIND_SURFACE_CONTEXT</a> context, <a href="#FS_SEARCH_LEVEL">FS_SEARCH_LEVEL</a> level)</code>
  </dd>
  <dt>Summary</dt>
  <dd>Sets lateral extension to the given <code>level</code>. The value must be one of <code><a href="#FS_SEARCH_LEVEL">enum FS_SEARCH_LEVEL</a></code> values. The default value is <code><a href="#FS_SEARCH_LEVEL">FS_LEVEL_5</a></code>.</dd>
</dl>




### `getLateralExtension`

<dl>
  <dt>Signature</dt>
  <dd>
    <code><a href="#FS_SEARCH_LEVEL">FS_SEARCH_LEVEL</a> getLateralExtension(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code>
  </dd>
  <dt>Return</dt>
  <dd>The lateral extension value that is currently set to the context.</dd>
</dl>




### `setSmartConversionOptions`

<dl>
  <dt>Signature</dt>
  <dd>
    <code>void setSmartConversionOptions(<a href="#context">FIND_SURFACE_CONTEXT</a> context, int options)</code>
  </dd>
  <dt>Summary</dt>
  <dd>Sets <a href="https://github.com/CurvSurf/FindSurface#smart-conversion">smart conversion</a> options to the given <code>options</code>, which can be any bit-OR combinations of <code><a href="#FS_SMART_CONVERSION_OPTION">FS_SMART_CONVERSION_OPTION</a></code> values. The default value is <code><a href="#FS_SMART_CONVERSION_OPTION">FS_SCO_NONE</a></code>.</dd>.
</dl>




### `getSmartConversionOptions`

<dl>
  <dt>Signature</dt>
  <dd>
    <code>int getSmartConversionOptions(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code>
  </dd>
  <dt>Return</dt>
  <dd>The <a href="https://github.com/CurvSurf/FindSurface#smart-conversion">smart conversion</a> option value that is currently set to the context.</dd>
</dl>




## Algorithm



### `findSurface`

<dl>
  <dt>Signature</dt>
  <dd>
    <code><a href="#FS_ERROR">FS_ERROR</a> findSurface(<a href="#context">FIND_SURFACE_CONTEXT</a> context, <a href="#FS_FEATURE_TYPE">FS_FEATURE_TYPE</a> type, unsigned int start_index, float touchRadius, <a href="#FS_FEATURE_RESULT">FS_FEATURE_RESULT</a> *result)</code>
  </dd>
  <dt>Summary</dt>
  <dd>
    Run FindSurface algorithm on the point array that have been set to <code><a href="#setPointCloudFloat/Double">setPointCloudFloat/Double</a></code> functions. The algorithm searches the points for a specific geometry surface represented by <code>type</code> around the point of which index is <code>start_index</code> (seed index) in the point array. <code>touchRadius</code> (seed radius) defines the initial search space and must be positive a non-zero value. The result will be written in the <code><a href="#FS_FEATURE_RESULT">struct FS_FEATURE_RESULT</a></code> pointed by <code>result</code>.
  </dd>
  <dt>Return</dt>
  <dd>An error code of <code><a href="#FS_ERROR">FS_ERROR</a></code> with value of <code><a href="#FS_ERROR">FS_NO_ERROR</a></code> indicating if succeeded to create the context. Otherwise, error codes corresponding to the reason of the failure is returned.</dd>
  <dt>Possible errors</dt>
	<dd>
    <code><a href="#FS_ERROR">FS_OUT_OF_MEMORY</a></code> if the system fails to allocate memory for intermediate data arrays.</dd> 
  <dd>
    <code><a href="#FS_ERROR">FS_INVALID_OPERATION</a></code> if one of the following is true:
    <ul>
      <li>one of the parameters is in an invalid state;</li>
      <li>pointcloud input is in an invalid state;</li>
      <li><code>type</code> is <code><a href="#FS_FEATURE_TYPE">FS_TYPE_NONE</a></code>;</li>
      <li><code>start_index</code> is greater than or equal to the number of the given points;</li>
      <li><code>touchRadius</code> is zero or a negative number;</li>
      <li><code>result</code> is NULL.</li>
    </ul>
  </dd>
</dl>




### Specializations of [`findSurface`](#findsurface)

The following variations are specializations of [`findSurface`](#findsurface) function, which is designed to search for surfaces of particular feature types.



#### `findStripPlane`

<dl>
  <dt>Signature</dt>
  <dd>
    <code><a href="#FS_ERROR">FS_ERROR</a> findStripPlane(<a href="#context">FIND_SURFACE_CONTEXT</a> context, unsigned int index_1, unsigned int index_2, float touchRadius, <a href="#FS_FEATURE_RESULT">FS_FEATURE_RESULT</a> *result)</code>
  </dd>
  <dt>Summary</dt>
  <dd>A specialization of <code><a href="#findsurface">findSurface</a></code> function to search for a strip plane (long and narrow) using two seed points (<code>index_1</code> and <code>index_2</code>) on the plane. <code>touchRadius</code> should be approximately half of the thickness of the target geometry for better results. Refer to the <code><a href="#findsurface">findSurface</a></code> function reference for the common details.</dd>
</dl>




#### `findRodCylinder`

<dl>
  <dt>Signature</dt>
  <dd>
    <code><a href="#FS_ERROR">FS_ERROR</a> findRodCylider(<a href="#context">FIND_SURFACE_CONTEXT</a> context, unsigned int index_1, unsigned int index_2, float touchRadius, <a href="#FS_FEATURE_RESULT">FS_FEATURE_RESULT</a> *result)</code>
  </dd>
  <dt>Summary</dt>
  <dd>A specialization of <code><a href="#findsurface">findSurface</a></code> function to search for a rod cylinder (long and narrow) using two seed points (<code>index_1</code> and <code>index_2</code>) on the lateral surface. <code>touchRadius</code> should be approximately half of the thickness of the target geometry for better results. Refer to the <code><a href="#findsurface">findSurface</a></code> function reference for the common details.</dd>
</dl>




#### `findDiskCylinder`

<dl>
  <dt>Signature</dt>
  <dd>
    <code><a href="#FS_ERROR">FS_ERROR</a> findDiskCylinder(<a href="#context">FIND_SURFACE_CONTEXT</a> context, unsigned int index_1, unsigned int index_2, unsigned int index_3, float touchRadius, <a href="#FS_FEATURE_RESULT">FS_FEATURE_RESULT</a> *result)</code>
  </dd>
  <dt>Summary</dt>
  <dd>A specialization of <code><a href="#findsurface">findSurface</a></code> function to search for a disk cylinder (thin and broad) using three seed points (<code>index_1</code>, <code>index_2</code> and <code>index_3</code>) on the lateral surface. <code>touchRadius</code> should be approximately half of the thickness of the target geometry for better results. Refer to the <code><a href="#findsurface">findSurface</a></code> function reference for the common details.</dd>
</dl>




#### `findDiskCone`

<dl>
  <dt>Signature</dt>
  <dd>
    <code><a href="#FS_ERROR">FS_ERROR</a> findDiskCone(<a href="#context">FIND_SURFACE_CONTEXT</a> context, unsigned int index_1, unsigned int index_2, unsigned int index_3, float touchRadius, <a href="#FS_FEATURE_RESULT">FS_FEATURE_RESULT</a> *result)</code>
  </dd>
  <dt>Summary</dt>
  <dd>A specialization of <code><a href="#findsurface">findSurface</a></code> function to search for a disk cone (thin and broad) using three seed points (<code>index_1</code>, <code>index_2</code> and <code>index_3</code>) on the lateral surface. <code>touchRadius</code> should be approximately half of the thickness of the target geometry for better results. Refer to the <code><a href="#findsurface">findSurface</a></code> function reference for the common details.</dd>
</dl>




#### `findThinRingTorus`

<dl>
  <dt>Signature</dt>
  <dd>
    <code><a href="#FS_ERROR">FS_ERROR</a> findThinRingTorus(<a href="#context">FIND_SURFACE_CONTEXT</a> context, unsigned int index_1, unsigned int index_2, unsigned int index_3, float touchRadius, <a href="#FS_FEATURE_RESULT">FS_FEATURE_RESULT</a> *result)</code>
	</dd>
  <dt>Summary</dt>
  <dd>A specialization of <code><a href="#findsurface">findSurface</a></code> function to search for a thin ring torus using three seed points (<code>index_1</code>, <code>index_2</code> and <code>index_3</code>) on the surface. <code>touchRadius</code> should be approximately half of the thickness of the target geometry for better results. Refer to the <code><a href="#findsurface">findSurface</a></code> function reference for the common details.</dd>
</dl>




## Miscellaneous



### `getInOutlierFlags`

<dl>
  <dt>Signature</dt>
  <dd>
    <code>const unsigned char *getInOutlierFlags(<a href="#context">FIND_SURFACE_CONTEXT</a> context)</code>
  </dd>
  <dt>Summary</dt>
  <dd>
    Provides an array of which elements indicate whether the corresponding points are outliers. A non-zero value means outlier. Otherwise, it means inlier. The length of the array is the same with the input point array. 
  </dd>
  <dt>Note</dt>
  <dd>The returned array must not be modified or deallocated because the returned pointer points to an internal buffer array in the context.</dd>
</dl>




[RMS]: https://github.com/CurvSurf/FindSurface#what-exactly-do-i-get-from-findsurface
[PARAM]: https://github.com/CurvSurf/FindSurface#how-does-it-work