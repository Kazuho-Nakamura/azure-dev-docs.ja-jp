---
ms.openlocfilehash: ab31ee32ea940db2d7bcfa2fe36475d8a648bfc9
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2019
ms.locfileid: "68284353"
---
| **仮想マシンの作成** || 
|---|---|
| [仮想マシンの管理][1] | 仮想マシンを作成、変更、開始、停止、削除します。 |
| [カスタム イメージからの仮想マシンの作成][2] | 仮想マシンのカスタム イメージを作成し、そのイメージを使用して新しい仮想マシンを作成します。 | 
| [スナップショットから特殊化された VHD を使って仮想マシンを作成する][3] | 仮想マシンの OS とデータ ディスクからスナップショットを作成し、そのスナップショットからマネージド ディスクを作成した後、マネージド ディスクをアタッチすることにより仮想マシンを作成します。 |  
| [同一ネットワーク内に同時に仮想マシンを作成する][4] | 2 つのサブネットがある同一仮想ネットワーク上の同一リージョンに、並行して複数の仮想マシンを作成します。 |
| [複数のリージョンに対して同時に仮想マシンを作成する][5] | 複数の Azure リージョンにわたって一連の仮想マシンを作成し、負荷分散します。 |
| **ネットワークの仮想マシン** || 
| [仮想ネットワークを管理する][6] | 2 つのサブネットがある仮想ネットワークを設定し、これらに対するインターネット アクセスを制限します。 |
| **スケール セットの作成** ||
| [ロード バランサーを使って仮想マシン スケール セットを作成する][7] | VM スケール セットを作成してロード バランサーを設定し、スケール セットの VM への SSH 接続文字列を取得します。 |

[1]: ../java-sdk-manage-virtual-machines.md
[2]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-custom-image/
[3]: https://azure.microsoft.com/resources/samples/managed-disk-java-create-virtual-machine-using-specialized-disk-from-vhd/
[4]: https://azure.microsoft.com/resources/samples/compute-java-manage-virtual-machines-in-parallel/
[5]: ../java-sdk-virtual-machines-in-parallel.md
[6]: ../java-sdk-manage-virtual-networks.md
[7]: ../java-sdk-manage-vm-scalesets.md