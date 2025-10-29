import { Badge } from "./ui/badge";
import { Button } from "./ui/button";
import { Card } from "./ui/card";
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "./ui/table";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "./ui/tooltip";
import { MoreVertical, TrendingDown, TrendingUp } from "lucide-react";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "./ui/dropdown-menu";

interface ProductionLine {
  id: string;
  name: string;
  status: "operational" | "maintenance" | "offline";
  output: number;
  target: number;
  efficiency: number;
  lastUpdate: string;
}

const productionLines: ProductionLine[] = [
  {
    id: "L001",
    name: "Assembly Line A1",
    status: "operational",
    output: 1247,
    target: 1200,
    efficiency: 96.2,
    lastUpdate: "2 min ago",
  },
  {
    id: "L002",
    name: "Assembly Line A2",
    status: "operational",
    output: 1189,
    target: 1200,
    efficiency: 94.1,
    lastUpdate: "5 min ago",
  },
  {
    id: "L003",
    name: "Paint Line P1",
    status: "operational",
    output: 842,
    target: 900,
    efficiency: 91.5,
    lastUpdate: "1 min ago",
  },
  {
    id: "L004",
    name: "Paint Line P2",
    status: "maintenance",
    output: 0,
    target: 900,
    efficiency: 0,
    lastUpdate: "1 hour ago",
  },
  {
    id: "L005",
    name: "Welding Line W1",
    status: "operational",
    output: 1523,
    target: 1400,
    efficiency: 98.7,
    lastUpdate: "3 min ago",
  },
  {
    id: "L006",
    name: "Welding Line W2",
    status: "operational",
    output: 1401,
    target: 1400,
    efficiency: 97.3,
    lastUpdate: "4 min ago",
  },
  {
    id: "L007",
    name: "QC Station Q1",
    status: "operational",
    output: 2847,
    target: 2800,
    efficiency: 99.1,
    lastUpdate: "1 min ago",
  },
  {
    id: "L008",
    name: "QC Station Q2",
    status: "offline",
    output: 0,
    target: 2800,
    efficiency: 0,
    lastUpdate: "3 hours ago",
  },
];

function getStatusBadge(status: ProductionLine["status"]) {
  switch (status) {
    case "operational":
      return <Badge variant="outline" className="bg-chart-3/20 text-chart-3 border-0">Operational</Badge>;
    case "maintenance":
      return <Badge variant="outline" className="bg-chart-4/20 text-chart-4 border-0">Maintenance</Badge>;
    case "offline":
      return <Badge variant="outline" className="bg-destructive/20 text-destructive border-0">Offline</Badge>;
  }
}

export function ProductionTable() {
  return (
    <Card className="p-6 bg-card shadow-elevation-sm border-0">
      <div className="mb-6">
        <h3 className="text-foreground">Production Line Status</h3>
        <p className="text-muted-foreground mt-1">
          Real-time monitoring of all production lines
        </p>
      </div>
      
      <div className="rounded-lg overflow-hidden">
        <Table>
          <TableHeader>
            <TableRow className="bg-muted">
              <TableHead className="table-header">Line ID</TableHead>
              <TableHead className="table-header">Name</TableHead>
              <TableHead className="table-header">Status</TableHead>
              <TableHead className="table-header text-right">Output</TableHead>
              <TableHead className="table-header text-right">Target</TableHead>
              <TableHead className="table-header text-right">Efficiency</TableHead>
              <TableHead className="table-header">Last Update</TableHead>
              <TableHead className="table-header text-right">Actions</TableHead>
            </TableRow>
          </TableHeader>
          <TableBody>
            {productionLines.map((line) => {
              const performanceGap = line.output - line.target;
              const isOverPerforming = performanceGap > 0;
              
              return (
                <TableRow key={line.id} className="hover:bg-muted/50">
                  <TableCell>
                    <span className="text-primary">{line.id}</span>
                  </TableCell>
                  <TableCell className="text-foreground">{line.name}</TableCell>
                  <TableCell>{getStatusBadge(line.status)}</TableCell>
                  <TableCell className="text-right text-foreground">
                    {line.output.toLocaleString()}
                  </TableCell>
                  <TableCell className="text-right text-foreground">
                    {line.target.toLocaleString()}
                  </TableCell>
                  <TableCell className="text-right">
                    <TooltipProvider>
                      <Tooltip>
                        <TooltipTrigger asChild>
                          <div className="flex items-center justify-end gap-1">
                            {line.status === "operational" && (
                              isOverPerforming ? (
                                <TrendingUp className="h-4 w-4 text-chart-3" />
                              ) : (
                                <TrendingDown className="h-4 w-4 text-muted-foreground" />
                              )
                            )}
                            <span className={line.efficiency >= 95 ? "text-chart-3" : "text-foreground"}>
                              {line.efficiency}%
                            </span>
                          </div>
                        </TooltipTrigger>
                        <TooltipContent>
                          <p>
                            {isOverPerforming ? "+" : ""}
                            {performanceGap} units vs target
                          </p>
                        </TooltipContent>
                      </Tooltip>
                    </TooltipProvider>
                  </TableCell>
                  <TableCell className="text-muted-foreground">
                    {line.lastUpdate}
                  </TableCell>
                  <TableCell className="text-right">
                    <DropdownMenu>
                      <DropdownMenuTrigger asChild>
                        <Button variant="ghost" size="icon">
                          <MoreVertical className="h-4 w-4" />
                        </Button>
                      </DropdownMenuTrigger>
                      <DropdownMenuContent align="end">
                        <DropdownMenuItem>View Details</DropdownMenuItem>
                        <DropdownMenuItem>View History</DropdownMenuItem>
                        <DropdownMenuItem>Schedule Maintenance</DropdownMenuItem>
                        <DropdownMenuItem>Generate Report</DropdownMenuItem>
                      </DropdownMenuContent>
                    </DropdownMenu>
                  </TableCell>
                </TableRow>
              );
            })}
          </TableBody>
        </Table>
      </div>
    </Card>
  );
}
