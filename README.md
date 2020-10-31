# Feats / Feature Store

`feats/feature-store` is a zero-dependency feature store available in several languages (see below). 

## What is `feats/feature-store`?

`feats/feature-store` allows you to persist your features whereever you want and to deserialize them to whatever data input format you need.

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

// use the 
await model.fit(inputTensor, labelTensor)
```

Learn more e.g. Point-in-Time correctness, offline-online feature values

## Why `feats/feature-store`?

- ✅ **Zero Infrastructure Dependencies**: Use with any storage, database, message broker you want
- ✅ **Zero Tooling Dependencies**: Use with any ML framework you want
- ✅ **Zero Runtime Dependencies**: Use locally, in notebooks, in the cloud, or whereever you want
- ✅ **Zero Org Dependencies**: Use with whatever ML/Data Science workflows and teams you want

This means for you:

- ✅ No setup, maintainance, monitoring, outages
- ✅ No new platform or infrastructure to learn
- ✅ No platform, vendor, tooling lock-in
- ✅ No unsupported workflows; extend, integrate, and make it fit your needs

## Usage

`feats/feature-store` is available for multiple languages.

> You can use different languages for persisting and deserializing, e.g. you can persist via `feats/feature-store-ts` and deserialize via `feats/feature-store-py`.

- `Python`: [feats/feature-store-py](./)
- `Node/JS/TS`: [feats/feature-store-ts](./)
