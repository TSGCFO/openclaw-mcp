---
source_url: https://docs.openclaw.ai/tr/install/kubernetes
title: "Kubernetes - OpenClaw"
---

[Ana içeriğe atla](https://docs.openclaw.ai/tr/install/kubernetes#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/tr)

![TR](https://d3gk2c5xim1je2.cloudfront.net/flags/TR.svg)

Türkçe

Ara...

Ctrl K

Ara...

Navigation

Hosting

Kubernetes

[Get started](https://docs.openclaw.ai/tr) [Install](https://docs.openclaw.ai/tr/install) [Channels](https://docs.openclaw.ai/tr/channels) [Agents](https://docs.openclaw.ai/tr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tr/tools) [Models](https://docs.openclaw.ai/tr/providers) [Platforms](https://docs.openclaw.ai/tr/platforms) [Gateway & Ops](https://docs.openclaw.ai/tr/gateway) [Reference](https://docs.openclaw.ai/tr/cli) [Help](https://docs.openclaw.ai/tr/help)

Bu sayfada

- [Kubernetes üzerinde OpenClaw](https://docs.openclaw.ai/tr/install/kubernetes#kubernetes-%C3%BCzerinde-openclaw)
- [Neden Helm değil?](https://docs.openclaw.ai/tr/install/kubernetes#neden-helm-de%C4%9Fil)
- [Gerekenler](https://docs.openclaw.ai/tr/install/kubernetes#gerekenler)
- [Hızlı başlangıç](https://docs.openclaw.ai/tr/install/kubernetes#h%C4%B1zl%C4%B1-ba%C5%9Flang%C4%B1%C3%A7)
- [Kind ile yerel test](https://docs.openclaw.ai/tr/install/kubernetes#kind-ile-yerel-test)
- [Adım adım](https://docs.openclaw.ai/tr/install/kubernetes#ad%C4%B1m-ad%C4%B1m)
- [1) Dağıtın](https://docs.openclaw.ai/tr/install/kubernetes#1-da%C4%9F%C4%B1t%C4%B1n)
- [2) Gateway’e erişin](https://docs.openclaw.ai/tr/install/kubernetes#2-gateway%E2%80%99e-eri%C5%9Fin)
- [Neler dağıtılır](https://docs.openclaw.ai/tr/install/kubernetes#neler-da%C4%9F%C4%B1t%C4%B1l%C4%B1r)
- [Özelleştirme](https://docs.openclaw.ai/tr/install/kubernetes#%C3%B6zelle%C5%9Ftirme)
- [Aracı yönergeleri](https://docs.openclaw.ai/tr/install/kubernetes#arac%C4%B1-y%C3%B6nergeleri)
- [Gateway yapılandırması](https://docs.openclaw.ai/tr/install/kubernetes#gateway-yap%C4%B1land%C4%B1rmas%C4%B1)
- [Sağlayıcı ekleyin](https://docs.openclaw.ai/tr/install/kubernetes#sa%C4%9Flay%C4%B1c%C4%B1-ekleyin)
- [Özel namespace](https://docs.openclaw.ai/tr/install/kubernetes#%C3%B6zel-namespace)
- [Özel kalıp](https://docs.openclaw.ai/tr/install/kubernetes#%C3%B6zel-kal%C4%B1p)
- [Port-forward ötesine açma](https://docs.openclaw.ai/tr/install/kubernetes#port-forward-%C3%B6tesine-a%C3%A7ma)
- [Yeniden dağıtım](https://docs.openclaw.ai/tr/install/kubernetes#yeniden-da%C4%9F%C4%B1t%C4%B1m)
- [Kaldırma](https://docs.openclaw.ai/tr/install/kubernetes#kald%C4%B1rma)
- [Mimari notları](https://docs.openclaw.ai/tr/install/kubernetes#mimari-notlar%C4%B1)
- [Dosya yapısı](https://docs.openclaw.ai/tr/install/kubernetes#dosya-yap%C4%B1s%C4%B1)
- [İlgili](https://docs.openclaw.ai/tr/install/kubernetes#i%CC%87lgili)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/tr/install/kubernetes\#kubernetes-%C3%BCzerinde-openclaw)  Kubernetes üzerinde OpenClaw

Kubernetes üzerinde OpenClaw çalıştırmak için asgari bir başlangıç noktası — üretime hazır bir dağıtım değildir. Temel kaynakları kapsar ve ortamınıza uyarlanması amaçlanır.

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#neden-helm-de%C4%9Fil)  Neden Helm değil?

OpenClaw birkaç yapılandırma dosyası olan tek bir kapsayıcıdır. İlginç özelleştirme altyapı şablonlamasında değil, aracı içeriğinde (Markdown dosyaları, Skills, yapılandırma geçersiz kılmaları) yer alır. Kustomize, Helm chart ek yükü olmadan overlay’leri yönetir. Dağıtımınız daha karmaşık hâle gelirse bu manifestlerin üzerine bir Helm chart katmanı eklenebilir.

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#gerekenler)  Gerekenler

- Çalışan bir Kubernetes kümesi (AKS, EKS, GKE, k3s, kind, OpenShift vb.)
- Kümenize bağlı `kubectl`
- En az bir model sağlayıcısı için API anahtarı

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#h%C4%B1zl%C4%B1-ba%C5%9Flang%C4%B1%C3%A7)  Hızlı başlangıç

```
# Sağlayıcınızla değiştirin: ANTHROPIC, GEMINI, OPENAI veya OPENROUTER
export <PROVIDER>_API_KEY="..."
./scripts/k8s/deploy.sh

kubectl port-forward svc/openclaw 18789:18789 -n openclaw
open http://localhost:18789
```

Control UI için yapılandırılmış paylaşılan gizli bilgiyi alın. Bu dağıtım betiği
varsayılan olarak token kimlik doğrulaması oluşturur:

```
kubectl get secret openclaw-secrets -n openclaw -o jsonpath='{.data.OPENCLAW_GATEWAY_TOKEN}' | base64 -d
```

Yerel hata ayıklama için `./scripts/k8s/deploy.sh --show-token`, dağıtımdan sonra token’ı yazdırır.

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#kind-ile-yerel-test)  Kind ile yerel test

Bir kümeniz yoksa [Kind](https://kind.sigs.k8s.io/) ile yerel olarak bir tane oluşturun:

```
./scripts/k8s/create-kind.sh           # docker veya podman'ı otomatik algılar
./scripts/k8s/create-kind.sh --delete  # kaldır
```

Ardından her zamanki gibi `./scripts/k8s/deploy.sh` ile dağıtın.

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#ad%C4%B1m-ad%C4%B1m)  Adım adım

### [​](https://docs.openclaw.ai/tr/install/kubernetes\#1-da%C4%9F%C4%B1t%C4%B1n)  1) Dağıtın

**Seçenek A** — ortamda API anahtarı (tek adım):

```
# Sağlayıcınızla değiştirin: ANTHROPIC, GEMINI, OPENAI veya OPENROUTER
export <PROVIDER>_API_KEY="..."
./scripts/k8s/deploy.sh
```

Betik API anahtarı ve otomatik üretilmiş bir Gateway token’ı içeren bir Kubernetes Secret oluşturur, ardından dağıtır. Secret zaten varsa mevcut Gateway token’ını ve değiştirilmemekte olan sağlayıcı anahtarlarını korur.**Seçenek B** — secret’ı ayrı oluşturun:

```
export <PROVIDER>_API_KEY="..."
./scripts/k8s/deploy.sh --create-secret
./scripts/k8s/deploy.sh
```

Yerel test için token’ın stdout’a yazdırılmasını istiyorsanız her iki komutla da `--show-token` kullanın.

### [​](https://docs.openclaw.ai/tr/install/kubernetes\#2-gateway%E2%80%99e-eri%C5%9Fin)  2) Gateway’e erişin

```
kubectl port-forward svc/openclaw 18789:18789 -n openclaw
open http://localhost:18789
```

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#neler-da%C4%9F%C4%B1t%C4%B1l%C4%B1r)  Neler dağıtılır

```
Namespace: openclaw (OPENCLAW_NAMESPACE ile yapılandırılabilir)
├── Deployment/openclaw        # Tek pod, init kapsayıcı + gateway
├── Service/openclaw           # 18789 portunda ClusterIP
├── PersistentVolumeClaim      # Aracı durumu ve yapılandırması için 10Gi
├── ConfigMap/openclaw-config  # openclaw.json + AGENTS.md
└── Secret/openclaw-secrets    # Gateway token + API anahtarları
```

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#%C3%B6zelle%C5%9Ftirme)  Özelleştirme

### [​](https://docs.openclaw.ai/tr/install/kubernetes\#arac%C4%B1-y%C3%B6nergeleri)  Aracı yönergeleri

`scripts/k8s/manifests/configmap.yaml` içindeki `AGENTS.md` dosyasını düzenleyin ve yeniden dağıtın:

```
./scripts/k8s/deploy.sh
```

### [​](https://docs.openclaw.ai/tr/install/kubernetes\#gateway-yap%C4%B1land%C4%B1rmas%C4%B1)  Gateway yapılandırması

`scripts/k8s/manifests/configmap.yaml` içindeki `openclaw.json` dosyasını düzenleyin. Tam başvuru için [Gateway yapılandırması](https://docs.openclaw.ai/tr/gateway/configuration) sayfasına bakın.

### [​](https://docs.openclaw.ai/tr/install/kubernetes\#sa%C4%9Flay%C4%B1c%C4%B1-ekleyin)  Sağlayıcı ekleyin

Ek anahtarları dışa aktarıp yeniden çalıştırın:

```
export ANTHROPIC_API_KEY="..."
export OPENAI_API_KEY="..."
./scripts/k8s/deploy.sh --create-secret
./scripts/k8s/deploy.sh
```

Üzerine yazmadığınız sürece mevcut sağlayıcı anahtarları Secret içinde kalır.Veya Secret’ı doğrudan yamalayın:

```
kubectl patch secret openclaw-secrets -n openclaw \
  -p '{"stringData":{"<PROVIDER>_API_KEY":"..."}}'
kubectl rollout restart deployment/openclaw -n openclaw
```

### [​](https://docs.openclaw.ai/tr/install/kubernetes\#%C3%B6zel-namespace)  Özel namespace

```
OPENCLAW_NAMESPACE=my-namespace ./scripts/k8s/deploy.sh
```

### [​](https://docs.openclaw.ai/tr/install/kubernetes\#%C3%B6zel-kal%C4%B1p)  Özel kalıp

`scripts/k8s/manifests/deployment.yaml` içindeki `image` alanını düzenleyin:

```
image: ghcr.io/openclaw/openclaw:latest # veya https://github.com/openclaw/openclaw/releases adresinden belirli bir sürüme sabitleyin
```

### [​](https://docs.openclaw.ai/tr/install/kubernetes\#port-forward-%C3%B6tesine-a%C3%A7ma)  Port-forward ötesine açma

Varsayılan manifestler Gateway’i pod içinde loopback’e bağlar. Bu, `kubectl port-forward` ile çalışır ancak pod IP’sine ulaşması gereken bir Kubernetes `Service` veya Ingress yolu ile çalışmaz.Gateway’i bir Ingress veya yük dengeleyici üzerinden açmak istiyorsanız:

- `scripts/k8s/manifests/configmap.yaml` içindeki Gateway bağlamasını `loopback`’ten dağıtım modelinize uyan loopback dışı bir bağlamaya değiştirin
- Gateway kimlik doğrulamasını etkin tutun ve uygun TLS sonlandırmalı bir giriş noktası kullanın
- Gerekli olduğunda desteklenen web güvenlik modelini kullanarak Control UI’yi uzak erişim için yapılandırın (örneğin HTTPS/Tailscale Serve ve açık izin verilen origin’ler)

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#yeniden-da%C4%9F%C4%B1t%C4%B1m)  Yeniden dağıtım

```
./scripts/k8s/deploy.sh
```

Bu, tüm manifestleri uygular ve herhangi bir yapılandırma veya secret değişikliğini almak için pod’u yeniden başlatır.

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#kald%C4%B1rma)  Kaldırma

```
./scripts/k8s/deploy.sh --delete
```

Bu, PVC dahil olmak üzere namespace’i ve içindeki tüm kaynakları siler.

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#mimari-notlar%C4%B1)  Mimari notları

- Gateway varsayılan olarak pod içinde loopback’e bağlanır, bu nedenle dahil edilen kurulum `kubectl port-forward` içindir
- Küme kapsamlı kaynak yoktur — her şey tek bir namespace içinde yaşar
- Güvenlik: `readOnlyRootFilesystem`, `drop: ALL` yetenekleri, root olmayan kullanıcı (UID 1000)
- Varsayılan yapılandırma Control UI’yi daha güvenli yerel erişim yolunda tutar: loopback bağlama artı `http://127.0.0.1:18789` için `kubectl port-forward`
- localhost erişiminin ötesine geçerseniz desteklenen uzak modeli kullanın: HTTPS/Tailscale artı uygun Gateway bağlama ve Control UI origin ayarları
- Secret’lar geçici bir dizinde üretilir ve doğrudan kümeye uygulanır — hiçbir gizli bilgi depo çalışma kopyasına yazılmaz

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#dosya-yap%C4%B1s%C4%B1)  Dosya yapısı

```
scripts/k8s/
├── deploy.sh                   # Namespace + secret oluşturur, kustomize ile dağıtır
├── create-kind.sh              # Yerel Kind kümesi (docker/podman'ı otomatik algılar)
└── manifests/
    ├── kustomization.yaml      # Kustomize tabanı
    ├── configmap.yaml          # openclaw.json + AGENTS.md
    ├── deployment.yaml         # Güvenlik sağlamlaştırmalı pod tanımı
    ├── pvc.yaml                # 10Gi kalıcı depolama
    └── service.yaml            # 18789 üzerinde ClusterIP
```

## [​](https://docs.openclaw.ai/tr/install/kubernetes\#i%CC%87lgili)  İlgili

- [Docker](https://docs.openclaw.ai/tr/install/docker)
- [Docker VM çalışma zamanı](https://docs.openclaw.ai/tr/install/docker-vm-runtime)
- [Kuruluma genel bakış](https://docs.openclaw.ai/tr/install)

[Hostinger](https://docs.openclaw.ai/tr/install/hostinger) [Linux Server](https://docs.openclaw.ai/tr/vps)

Ctrl+I