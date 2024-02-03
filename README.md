## 大概的思路：

当涉及到DirectoryTableBase的PTE Address时，可以通过计算得出。根据页表自映射的信息，很容易确定对应的PTE Address是PXE加上自映射页表的索引乘以8所得。

	
static const uint64_t cr3_ptebase = self_mapidx * 8 + pxe_base;

我们的目标是比较每个pfn的pte address。如果相等，那就意味着该pfn是CR3所对应的物理地址。

	
if (cur_mmpfn->pte_address != cr3_ptebase) continue;

此外，根据前辈们的文章，可以得知MMPFN的第一项是经过加密处理的Eprocess

	
auto decrypted_eprocess = ((cur_mmpfn->flags | 0xF000000000000000) >> 0xd) | 0xFFFF000000000000;

有此得出EPROCESS和Dirbase

https://bbs.kanxue.com/thread-261778.htm win10 1909逆向（通过任意物理帧判断是否是CR3和解密得到所属EPROCESS

# enum_real_dirbase
![Image text](https://github.com/Rythorndoran/enum_real_dirbase/blob/master/QQ%E6%88%AA%E5%9B%BE20230818115205.png)
