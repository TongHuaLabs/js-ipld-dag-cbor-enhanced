# js-ipld-dag-cbor-enhanced

Enhanced [js-ipld-dag-cbor](https://github.com/ipld/js-ipld-dag-cbor) which allows linking DAG Nodes via CID strings

```js
const objWithLink = {
  foo: new CID('bafyreicaoyussrycqolu4k2iaxleu2uakjlq57tuxq3djxn4wnyfp4yk3y'),
};
const objWithStrLink = {
  foo: 'bafyreicaoyussrycqolu4k2iaxleu2uakjlq57tuxq3djxn4wnyfp4yk3y',
};
const s1 = dagCBOR.util.serialize(objWithLink, { stringLinks: true });
const s2 = dagCBOR.util.serialize(objWithStrLink, { stringLinks: true });

expect(s1).to.be.eql(s2);
```

## API

### `dagCBOR.util.serialize(obj, options)`

Encodes an object into IPLD CBOR form, replacing any CIDs found within the object to CBOR tags (with an id of `42`).

- `obj` (any): any object able to be serialized as CBOR
- `options`({stringLinks: boolean}): Set `stringLinks` to true to allow linking DAG Nodes via CID strings

Returns the serialized node.

### `dagCBOR.util.deserialize(serialized)`

Decodes an IPLD CBOR encoded representation, restoring any CBOR tags (id `42`) to CIDs.

- `serialized` (`Uint8Array` or `String`): a binary blob representing an IPLD CBOR encoded object.

Returns the deserialized object.

### `dagCBOR.util.configureDecoder([options])`

Configure the underlying CBOR decoder.

Possible values in the `options` argument are:

- `size` (`Number`, optional): the current heap size used in CBOR parsing, this may grow automatically as larger blocks are encountered up to `maxSize`. Default: `65536` (64Kb).
- `maxSize` (`Number`, optional): the maximum size the CBOR parsing heap is allowed to grow to before `dagCBOR.util.deserialize()` returns an error. Default: `67108864` (64Mb).
- `tags` (`Object`, optional): an object whose keys are CBOR tag numbers and values are transform functions that accept a `value` and return a decoded representation of that `value`.

The CBOR decoder uses a heap size that is a power of two. Setting `size` to a number other than a power of two will result in a heap using the next-largest power of two.

Calling `dagCBOR.util.configureDecoder()` with no arguments will reset to the default decoder `size`, `maxSize` and `tags`.

### `dagCBOR.util.cid(obj[, options,])`

Create a [CID](https://github.com/multiformats/js-cid) for the given unserialized object.

- `obj` (any): any object able to be serialized as CBOR
- `options` (`Object`):

  - `hashAlg` (`String`): a [registered multicodec](https://github.com/multiformats/multicodec/blob/master/table.csv) hash algorithm.
  - `hashLen` (`String`): an optional hash length
  - `version` (`Number`): CID version number, defaults to `1`

Returns a Promise with the created CID.

## Contribute

Feel free to join in. All welcome. Open an [issue](https://github.com/ipld/js-ipld-dag-cbor/issues)!

Check out our [contributing document](https://github.com/ipld/ipld/blob/master/contributing.md) for more information on how we work, and about contributing in general. Please be aware that all interactions related to IPLD are subject to the IPFS [Code of Conduct](https://github.com/ipfs/community/blob/master/code-of-conduct.md).

Small note: If editing the README, please conform to the [standard-readme](https://github.com/RichardLitt/standard-readme) specification.

## License

[MIT](LICENSE) © 2015 David Dias
