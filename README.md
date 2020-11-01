# Feats / Feature Store

> `feature-store` is the fastest way to a Feature Store. It makes no assumptions on your current infrastructure, tooling, and workflows. There is nothing to host, maintain, or monitor. Just install the library and use it to store your features today.

`feature-store` is a code library, available for [several languages](#usage), that allows you to persist your features wherever you want and to deserialize them to whatever data input format you need.

## Examples

`feature-store` is implemented in [many languages](#usage). The following examples are for `TypeScript`.

#### Persisting Feature Values

```typescript
import { AWS } from 'aws-sdk'
import { FeatureStore, FeatureValue } from '@feats/feature-store'

// create feature value(s)
const featureValue = FeatureValue.from({
  entity: {
    name: 'customer',
    value: '123'
  },
  feature: {
    name: 'basket_value_usd_cents',
    value: 2395
  },
  // optional
  eventTime: new Date(2020, 08, 23, 14, 05)
})

// instantiate feature store(s)
const redshift = new AWS.Redshift({ ...yourConfig })
const redshiftStore = new FeatureStore({ driver: redshift })

// persist feature value(s)
await redshiftStore.persist(featureValue)
```

Learn more e.g. register features, version features, and manage your features

#### Deserializing Feature Values

```typescript
import * as tf from '@tensorflow/tfjs'
import { AWS } from 'aws-sdk'
import { FeatureStore, FeatureValue } from '@feats/feature-store'

// instantiate feature store(s)
const redshift = new AWS.Redshift({ ...yourConfig })
const redshiftStore = new FeatureStore({ driver: redshift })

// load feature value(s)
const inputTensor = await redshiftStore.load([
  'customer/basket_value_usd_cents'
], tf.Tensor)

const labelTensor = await redshiftStore.load([
  'customer/age_in_years'
], tf.Tensor)

// use the constructed tf.Tensors
await model.fit(inputTensor, labelTensor)
```

Learn more e.g. Point-in-Time correctness, offline-online feature values

## Why `feature-store`?

- ✅ **Zero Infrastructure Dependencies**: Use with any storage, database, message broker you want
- ✅ **Zero Tooling Dependencies**: Use with any ML framework you want
- ✅ **Zero Runtime Dependencies**: Use locally, in notebooks, in the cloud, or whereever you want
- ✅ **Zero Org Dependencies**: Use with whatever ML/Data Science workflows and teams you want

This means for you:

- ✅ No setup, maintainance, monitoring, or downtimes
- ✅ No new platform or infrastructure to learn
- ✅ No platform, vendor, tooling lock-in
- ✅ No unsupported workflows; extend, integrate, and make it fit your needs

## Usage

Pick your `feature-store` implementation. See available languages below.

> You can use different languages for persisting and deserializing, e.g. you can persist via `feature-store-ts` and deserialize via `feature-store-py`.

- `Python`: [feature-store-py][ffs-py]
- `Node/JS/TS`: [feature-store-ts][ffs-node]

## Supported Persistence Systems

- **AWS**
  - DynamoDB (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
  - Redshift (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
  - S3 (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
- **GCP**
  - BigTable (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
  - Memorystore (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
  - BigQuery (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
  - CloudStorage (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
- **Kafka** (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
- **Redis** (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
- **Cassandra** (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
- **FileSystem** (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
- **SQLite** (available in: [`Python`][ffs-py], [`Node.js`][ffs-node])
- **indexedDB** (available in: [`Node.js`][ffs-node])

## Supported Deserialization Formats

- **`Python`**: [`Numpy`][numpy-website], [`Pandas`][pandas-website], [`Tensorflow`][ts-website], [`PyTorch`][pytorch-website], [and many more ...](./)
- **`Node.js`**: [`Tensorflow-js`][tsjs-website]

## Extend

Are you missing support for a persistence system or deserialization format? 

See this [this guide](./) and learn how to easily implement it yourself.

[ffs-py]: ./
[ffs-node]: ./
[numpy-website]: https://numpy.org/
[pandas-website]: https://pandas.pydata.org/
[ts-website]: https://www.tensorflow.org/
[tsjs-website]: https://www.tensorflow.org/js/
[pytorch-website]: https://pytorch.org/
